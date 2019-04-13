---
layout: default
title: Algorithms
---

# Algorithms

<!-- TOC -->

- [Complexity](#complexity)
- [Sorting Algorithms](#sorting-algorithms)
    - [Insertion Sort](#insertion-sort)
    - [Selection sort](#selection-sort)
    - [Binary insertion sort](#binary-insertion-sort)
    - [Bubblesort](#bubblesort)
    - [Mergesort](#mergesort)
    - [Quicksort](#quicksort)
    - [Heapsort](#heapsort)
    - [Counting sort](#counting-sort)
    - [Bucketsort](#bucketsort)
    - [Radix sort](#radix-sort)
- [Dynamic programming](#dynamic-programming)
    - [Longest common substring](#longest-common-substring)
    - [Rod cutting](#rod-cutting)
- [Graph algorithms](#graph-algorithms)
    - [DFS](#dfs)
    - [BFS](#bfs)
    - [Dijkstra](#dijkstra)
    - [Bellman-Ford](#bellman-ford)
    - [Johnson](#johnson)
    - [Prim](#prim)
    - [Kruskal](#kruskal)
    - [Topological sort](#topological-sort)
    - [Ford-Fulkerson](#ford-fulkerson)
- [Geometrical algorithms](#geometrical-algorithms)
    - [Segment intersection](#segment-intersection)
    - [Jarvis's March](#jarviss-march)
    - [Graham's scan](#grahams-scan)

<!-- /TOC -->

## Complexity 

$$f(n) \in O(g(n)) \Longleftrightarrow \exists N, c_1, c_2 >0 \text{ s.t. } \forall n > N: 0 \leq g(n) \leq k g(n) $$

More informally, we can define other asymptotic nation as follows: 

<center>
<img src="{{ site.imageurl }}note_img/asymptotic_notation.png" style="width:80%;"/>
</center>

In the analysis of algorithms, many assumptions about hardware and basic operations must be made, e.g array access is constant time. 

## Sorting Algorithms

- Sorting is important because we can search sorted arrays very efficiently, in $O(\log n)$ time.
- In the worst case, $\Theta(n)$ exchanges will be needed. 
- Comparison-based algorithms require at least $\Omega(n\lg n)$ comparisons.

### Insertion Sort

Maintain a sorted section of the array, then insert new items into the correct position.

```python
def insertion_sort(a):
    for i in range(1, len(a)):
        # assert first i positions sorted
        j = i - 1
        while j >= 0 and a[j] > a[j+1]:
            swap(j, j+1)
            j -= 1  
```

Inserting the last element needs at most $n-1$ comparisons and swaps. The second last element requires $n-2$...

$$T(n) = 2 (1 + 2 + \cdots + n - 1) = n(n-1)$$

So insertion sort is $O(n^2)$. That said, it has a very small constant term so is often faster than $O(n \lg n)$ algorithms for small *n*. It is stable as long as we only swap if the element is larger than the key.

### Selection sort 

At each iteration, find the minimum of the remaining array and swap it to the current index. 

```python
def selection_sort(a):
    for i in range(len(a)):
        swap(i, argmin(a[i:end]))
```

$O(n^2)$ and unstable. Its only advantage is that it is easy to analyse.

### Binary insertion sort

Same as insertion sort, except we find the correct position using binary partitioning.

```python
def binary_insertion_sort(a):
    for i in range(1, len(a)):
        hi = i
        lo = 0
        
        while lo < hi:
            j = (hi + lo) // 2
            if a[i] > a[j]:
                lo = j + 1
            else:
                hi = j
```

Binary insertion sort will be preferred to insertion sort when comparisons are expensive, but the swapping costs mean that it is still $O(n^2)$.

### Bubblesort

In each pass, go through the list swapping adjacent elements as needed. If no swaps are done in a pass, the array is sorted. 

```python
def bubblesort(a):
    while True:
        didSwap = False
        for i in range(len(a) - 1):
            if a[i] > a[i+1]:
                swap(i, i+1)
                didSwap = True
        if not didSwap:
            break
```

In the worst case, an element will be *n* positions away from its final position, so the complexity is $O(n^2)$.

### Mergesort

### Quicksort

```python
def quicksort(a):
    pivot = a[len(a) - 1]
    i = 0
    j = len(a) - 2
    
    while i <= j:
        if a[i] > pivot and a[j] <= pivot:
            swap(i, j)
            i += 1
            j -= 1
        else if a[i] <= pivot:
            i += 1
        else: j -= 1
        
    # ASSERT i == j + 1
    # ASSERT all items to the left of i <= pivot
```

### Order statistics 

A quicksort-like algorithm can be used to compute the median and, more generally, **order statistics**.

1. Select a pivot and partition the array into subarrays of size $p$ and $n-p$
2. If $k < p$, recursively look for the kth item in the lower partition. 
3. Otherwise recurse into the upper partition to find rank $k-p$

This has recurrence:

$$T(n) = f(n/2) + kn = O(n)$$

However, the worst case is $O(n^2)$ as with quicksort. There exists a guaranteed linear time algorithm but it is much more complicated.

### Heapsort


### Counting sort 

Counting sort does not require comparisons. Assuming that the inputs are positive integers within some range, it counts the number of each element, then finds the cumulative sum, from which we can identify exactly where a given element should go.

```python
def counting_sort(a):
    count = [0] * max(a)
    for x in a:
        count[x] += 1
    # cumulate
    for i in range(1, len(count)):
        count[i] += count[i-1]
    
    sorted = [0] * len(a)
    for i in range(len(a) - 1, 0 included):
        idx = count[a[i]] - 1
        sorted[idx] = a[i]
        count[a[i]] -= 1
    return sorted
```

It is a stable sorting algorithm, with $\Theta(n)$ cost.

### Bucketsort

Bucketsort creates an array of buckets (linked lists), with the assumption that elements will fall into these buckets uniformly. We can then run insertion sort within each bucket before concatenating the buckets into a sorted array. 

```python
def bucket_sort(a):
    # assuming that elements are drawn uniformly from [0,1]
    n = len(a)
    bucketWidth = 1/n
    buckets = new [] of length n
    
    for x in a:
        idx = int(x / bucketWidth)
        bucket[idx].next = x
    
    sorted = []
    for b in buckets:
        insertion_sort(b)
        sorted.append(b)
    return b
```

We use insertion sort because each bucket should contain only one element on average. But the worst case is still $O(n^2)$ as a result.

### Radix sort


## Dynamic programming 

### Longest common substring

### Rod cutting

## Graph algorithms 

### DFS

### BFS

### Dijkstra

### Bellman-Ford

### Johnson


### Prim

### Kruskal


### Topological sort 


### Ford-Fulkerson


## Geometrical algorithms

### Segment intersection 

### Jarvis's March

### Graham's scan




