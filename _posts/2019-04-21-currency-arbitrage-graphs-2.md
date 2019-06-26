---
layout: post
title: Graph algorithms and currency arbitrage, part 2
---

In the [previous post]({{ site.baseurl }}{% post_url 2019-03-02-currency-arbitrage-graphs %}) we explored how graphs can be used to represent a currency market, and how we might use shortest-path algorithms to discover arbitrage opportunities. Today, we will apply this to real-world data. It should be noted that we are not attempting to build a functional arbitrage bot, but rather emphasise the graph-based methodology. Later on we'll discuss why this is unlikely to result in actionable arbitrage. 

We will be examining a market of cryptocurrencies. This is because it is much easier to acquire crypto order book data, and crypto markets are less liquid than fiat markets so we are much more likely to actually see arb.

Implicit in the above is the fact that we will focus on arbitrage within a single exchange. That is, we'll look to see if there are pathways between different coins on an exchange for which we can make money. This is easier than having to scrape data from multiple exchanges, but because it is easier, it is less likely to be profitable. That being said, it serves to illustrate the graph-based approach.

Obviously, markets are highly dynamic, with thousands of new bids and asks coming in each second. A proper arbitrage system needs to constantly be scanning for opportunities. However, this has added complications so we will just consider a single snapshot of data from the exchange.

With all this in mind, the overall implementation strategy was as follows:

1. For a given exchange, acquire the list of pairs that will form the vertices.
2. For each of these pairs, download a snapshot of the bid/ask.
3. Process these values accordingly, assigning them to directed edges on the graph. 
4. Using Bellman-Ford, find and return negative-weight cycles if they exist. 
5. Calculate the arbitrage that these negative-weight cycles correspond to.

*The full code for this project can be found in this [GitHub repo](https://github.com/robertmartin8/CryptoGraphArb/tree/master). If you find this post interesting, don't forget to leave a star!*

<script async defer src="https://buttons.github.io/buttons.js"></script>
<a class="github-button" href="https://github.com/robertmartin8/CryptoGraphArb" data-icon="octicon-star" data-size="large" aria-label="Star robertmartin8/MachineLearningStocks on GitHub">Star</a>

## Raw data 

For the raw data, I decided to use the [CryptoCompare API](https://min-api.cryptocompare.com/documentation) which has a load of free data compiled across multiple exchanges. To get started, you'll need to register to get a free API key.

As mentioned previously, we will only look at data from Binance. I chose Binance not because it has a large selection of altcoins, but because these altcoins can trade directly with multiple pairs (normally BTC, ETH, USDT, BNB). Some exchanges have many altcoins but you can only buy them with BTC – this is not well suited for arbitrage.


Firstly, we need to find out which pairs Binance offers. This is done with a simple call (where `AUTH` is your API key):

```python
import requests
import json 

def top_exchange_pairs():
    url = (
        "https://min-api.cryptocompare.com/data/v3/all/" + 
        "exchanges?topTier=true&api_key=" + AUTH
    )
    r = requests.get(url)
    with open("pairs_list.json", "w") as f:
        json.dump(r.json(), f)
```

This is an excerpt from the resulting JSON file. For each exchange, the `pairs` field lists all other coins that the key coin can be traded with.

```json
"Data":{  
   "Binance":{  
      "isActive":true,
      "isTopTier":true,
      "pairs":{  
         "ETH":["PAX", "TUSD", "USDT", "USDC", "BTC"],
         "ONGAS":["BTC", "BNB", "USDT"],
         "PHX":["ETH","BNB","BTC"]
      }
   },
   "Coinbase":{  
      "isActive":true,
      "isTopTier":true,
      "pairs":{  
         "ETH":["DAI", "USD", "USDC", "EUR", "GBP", "BTC"],
         "BCH":["BTC", "GBP", "EUR", "USD"]
      }
   }
}
```

I then filtered out coins with fewer than three tradable pairs. These coins are unlikely to participate in arbitrage – we would rather have a graph that is more connected.

```python
def binance_connected_pairs():
    with open("pairs_list.json", "r") as f:
        data = json.load(f)
    pairs = data["Data"]["Binance"]["pairs"]
    return {k: v for k, v in pairs.items() if len(v) > 3}
```

We are now ready to download a snapshot of the available exchange rates for each of these coins.

```python
import os
import tqdm  # progress bar

def download_snapshot(pair_dict, outfolder):
    if not os.path.exists(outfolder):
        os.makedirs(outfolder)

    # Download data and write to files
    for p1, p2s in tqdm(pair_dict.items()):
        url = (
            "https://min-api.cryptocompare.com/data/" 
            + f"ob/l1/top?fsyms={p1}&tsyms={','.join(p2s)}"
            + "&e=Binance&api_key=" + AUTH
        )
        r = requests.get(url)
        with open(f"{outfolder}/{p1}_pairs_snapshot.json", "w") as f:
            json.dump(r.json(), f)
```

We can then run all of the above functions to produce a directory full of the exchange rate data for the listed pairs.

```python
top_exchange_pairs()
connected = binance_connected_pairs()
download_snapshot(connected, "binance_data")
```

```json
"EOS": {
    "BNB": {
        "BID": ".2073",
        "ASK": ".2077"
    },
    "BTC": {
        "BID": ".0007632",
        "ASK": ".0007633"
    },
    "ETH": {
        "BID": ".02594",
        "ASK": ".025964"
    },
    "USDT": {
        "BID": "7.0441",
        "ASK": "7.046"
    },
    "PAX": {
        "BID": "7.0535",
        "ASK": "7.07"
    }
}
```

This excerpt reveals something that we glossed over completely in the previous post. As anyone who has tried to exchange currency on holiday will know, there are actually two exchange rates for a given currency pair depending on whether you are buying or selling the currency. In trading, these two prices are called the **bid** (the current highest price someone will buy for) and the **ask** (the current lowest price someone will sell for). As it happens, this is very easy to deal with in the context of graphs.

## Preparing the data

Having downloaded the raw data, we must now prepare it so that it can be put into a graph. This effectively means parsing it from the raw JSON and putting it into a pandas dataframe. We will arrange it in the dataframe such that it constitutes an adjacency matrix:

- Column ETH row BTC is the bid:
    - i.e someone will pay *x* BTC to buy my 1 ETH
    - this is then the weight of the ETH $\to$ BTC edge.
- Column BTC row ETH is the ask:
    - i.e I have to pay *y* BTC to buy someone's 1 ETH
    - the reciprocal of this is the weight of the BTC $\to$ ETH edge. 

I chose this particular row-column scheme because it results in intuitive indexing: `df.X.Y` is the amount of Y gained by selling 1 unit of X, and `df.A.B * df.B.C * df.C.D` is the total amount of D gained by trading 1 unit of A when trading via $A \to B \to C \to D$.

The column headers will be the same as the row headers, consisting of all the coins we are considering. The function that creates the adjacency matrix is shown here:

```python
def create_adj_matrix(pair_dict, folder, outfile="snapshot.csv"):
    # Union of 'from' and 'to' pairs
    flatten = lambda l: [item for sublist in l for item in sublist]
    keys, vals = pair_dict.items()
    all_pairs = list(set(keys).union(flatten(values)))

    # Create empty df
    df = pd.DataFrame(columns=all_pairs, index=all_pairs)

    for p1 in pair_dict.keys():
        with open(f"{folder}/{p1}_pairs_snapshot.json", "r") as f:
            res = json.load(f)
        quotes = res["Data"]["RAW"][p1]
        for p2 in quotes:
            try:
                df[p1][p2] = float(quotes[p2]["BID"])
                df[p2][p1] = 1 / float(quotes[p2]["ASK"])
            except KeyError:
                print(f"Error for {p1}/{p2}")
                continue
    df.to_csv(outfile)
```

## Putting the data into a graph

We will be using the [NetworkX](https://networkx.github.io/documentation/stable/) package, an intuitive yet extremely well documented library for dealing with all things graph-related in python. 

In particular, we will be using `nx.DiGraph`, which is just a (weighted) directed graph. I was initially concerned that it'd be difficult to get the data in: python libraries often adopt their own weird conventions and you have to modify your data so that is in the correct format. This was not really the case with NetworkX, it turns out that we already did most of the hard work when we put the data into our pandas adjacency matrix.

Firstly, we take negative logs as discussed in the [previous post]({{ site.baseurl }}{% post_url 2019-03-02-currency-arbitrage-graphs %}). Secondly, in our dataframe we currently have `NaN` whenever there is no edge between two vertices. To make a valid `nx.DiGraph`, we need to set these to zero. Lastly, we must transpose the dataframe because NetworkX uses a different row/column convention to me. We then pass this processed dataframe into the `nx.Digraph` constructor. Summarised in one line:

```python
g = nx.DiGraph(-np.log(df).fillna(0).T)
```

## Bellman-Ford

To implement Bellman-Ford, we make use of the funky `defaultdict` data structure. As the name suggests, it works exactly like a python dict, except that if you query a key that is not present you get a certain default value back. The first part of our implementation is quite standard, as we are just doing the $n - 1$ relaxations.

But because the 'classic' Bellman-Ford does not actually return negative-weight cycles, the second part of our implementation is a bit more complicated. The idea is that if after $n-1$ relaxations, there is an edge that can be relaxed further, then that edge must be on a negative weight cycle. So to find this cycle we walk back along the predecessors until a cycle is found, then return the cyclic portion of that walk. In order to prevent subsequent redundancy, we mark these vertices as 'seen' via another `defaultdict`. This procedure adds a linear cost to Bellman-Ford since we have to iterate over all the edges, but the asymptotic complexity overall remains $O(VE)$.


```python 
def bellman_ford_return_cycle(g, s):
    n = len(g.nodes())
    d = defaultdict(lambda: math.inf)  # distances dict
    p = defaultdict(lambda: -1)  # predecessor dict
    d[s] = 0

    for _ in range(n - 1):
        for u, v in g.edges():
            # Bellman-Ford relaxation
            weight = g[u][v]["weight"]
            if d[u] + weight < d[v]:
                d[v] = d[u] + weight
                p[v] = u  # update pred

        # Find cycles if they exist
        all_cycles = []
        seen = defaultdict(lambda: False)

        for u, v in g.edges():
            if seen[v]:
                continue
            # If we can relax further there must be a neg-weight cycle
            weight = g[u][v]["weight"]
            if d[u] + weight < d[v]:
                cycle = []
                x = v
                while True:
                    # Walk back along preds until a cycle is found
                    seen[x] = True
                    cycle.append(x)
                    x = p[x]
                    if x == v or x in cycle:
                        break
                # Slice to get the cyclic portion
                idx = cycle.index(x)
                cycle.append(x)
                all_cycles.append(cycle[idx:][::-1])
        return all_cycles
```

If the graph doesn't contain any negative-weight cycles, the function returns an empty list. As a reminder, this function returns all negative-weight cycles reachable from a given source vertex. To find all negative-weight cycles, we can simply call the above procedure on every vertex then eliminate duplicates.

```python
def all_negative_cycles(g):
    all_paths = []
    for v in g.nodes():
        all_paths.append(bellman_ford_negative_cycles(g, v))
    flattened = [item for sublist in all_paths for item in sublist]
    return [list(i) for i in set(tuple(j) for j in flattened)]
```

## Tying it all together 

The last thing we need is a function that calculates the value of an arbitrage given a negative-weight cycle on a graph. This is easy to implement: we just find the total weight along the path then exponentiate the negative total (because we have negative log weights).

```python
def calculate_arb(cycle, g, verbose=True):
    total = 0
    for (p1, p2) in zip(cycle, cycle[1:]):
        total += g[p1][p2]["weight"]
    arb = np.exp(-total) - 1
    if verbose:
        print("Path:", cycle)
        print(f"{arb*100:.2g}%\n")
    return arb
```

```python
def find_arbitrage(filename="snapshot.csv"):
    df = pd.read_csv(filename, header=0, index_col=0)
    g = nx.DiGraph(-np.log(df).fillna(0).T)

    if nx.negative_edge_cycle(g):
        print("ARBITRAGE FOUND\n" + "=" * 15 + "\n")
        for p in all_negative_cycles(g):
            calculate_arb(p, g)
    else:
        print("No arbitrage opportunities")
```

Running this function gives the following output:

```
ARBITRAGE FOUND
===============

Path: ['USDT', 'BAT', 'BTC', 'BNB', 'ZEC', 'USDT']
0.087%

Path: ['BTC', 'XRP', 'USDT', 'BAT', 'BTC']
0.05%

Path: ['BTC', 'BNB', 'ZEC', 'USDT', 'BAT', 'BTC']
0.087%

Path: ['BNB', 'ZEC', 'USDT', 'BAT', 'BTC', 'BNB']
0.087%

Path: ['USDT', 'BAT', 'BTC', 'XRP', 'USDT']
0.05%
```

0.09% is not exactly a huge money, but it is still risk-free profit, right? 

## Why wouldn't this work?

Notice that we haven't mentioned exchange fees at any point. In fact, Binance charges a standard 0.1% commission on every trade. It is easy to modify our code to incorporate this, we just multiply each rate by 0.999, but we don't need to compute anything to see that we would certainly be losing much more money than gained from the arbitrage. 

Secondly, it is likely that this whole analysis is flawed because of the way the data was collected. The function `download_snapshot` makes a request for each coin in sequence, taking a few seconds in total. But in these few seconds, prices may move - so really the above "arbitrage" may just be a result of our algorithm selecting some of the price movements. This could be fixed by using timestamps provided by the exchange to ensure that we are looking at the order book for each pair at the exact same moment in time.

Thirdly, we have assumed that you can trade an infinite quantity of the bid and ask. An order consists of a price and a quantity, so we will only be able to fill a limited quantity at the ask price. Thus in practice we would have to look at the top few levels.

It is not difficult to extend our methodology to arb between different exchanges. We would just need to aggregate the top of the order book from each exchange, then put the best bid/ask onto the respective edges. Of course, to do run this strategy live would require us to manage our inventory not just on a currency level but per currency per exchange, and factors like the congestion of the bitcoin network would come into play. 

Lastly, this analysis has only been for a single snapshot. A proper arbitrage bot would have to constantly look at all the order books. I think this could be done by having a websocket stream which keeps the graph updated with the latest quotes, and using a more advanced method for finding negative-weight cycles that does not involve recomputing the shortest paths via Bellman-Ford.


## Conclusion

All this begs the question: why is it so hard to find arbitrage? The simple answer is that other people are doing it smarter, better, and (more importantly) faster. With highly optimised algorithms (probably implemented in C++), 'virtual colocation' of servers, and proper networking software/hardware, professional market makers are able to exploit these simple arbitrage opportunities extremely rapidly.

In any case, the point of this post was not to develop a functional arbitrage bot but rather to demonstrate the power of graph algorithms in a non-standard use case. Hope you found it as interesting as I did!



