---
layout: default
title: Standard ML problems and solutions
---

# Standard ML exercises and solutions 

This document represents the majority of my revision for ML, as part of the Cambridge Computer Science Tripos Paper 1. For each problem, I present a brief description, my solution, the official solution if it exists and is significantly different to mine, and some remarks. In most cases, the solutions I offer represent my first attempt at the problem, so they may be flawed. My workflow was as follows:

1. Write a solution to the problem in VSCode and test it in PolyML.
2. If the solution works, copy it to this document and compare it with the actual solution. If there is a significant difference, add the actual solution and comment.
3. If the solution doesn't work, copy the code to this document but put an 'x' next to the name. Revisit the problem another time. If I still can't get it to work, look at the solution and note down the main issue. At a later date, try solving the problem again without the answers (i.e return to step 1). 

These problems have been taken from a number of sources, most notably [ML for the Working Programmer 2nd Edition](https://www.cl.cam.ac.uk/~lp15/MLbook/) and Cambridge's course notes for [Foundations of Computer Science]({{ site.url }}/notes/FoCS). 

General things that I have learnt: 

- Don't be afraid to write a stupid solution using if/else if you can't immediately figure out the one-liner.
 



## ML4WP Chapter 3 - Lists 

### 3.1 Maximum of list without pattern matching 

```ocaml
fun hd (x::_) = x
fun tl (_::xs) = xs

fun null [] = true
  | null (_::_) = false;

fun maxl ls: int = 
    if null (tl ls)  then (hd ls) else
    if (hd ls) > (hd (tl ls)) then maxl ((hd ls)::(tl (tl ls))) 
    else maxl ((hd (tl ls))::(tl (tl ls)));
```

### 3.2 Last element of list 

```ocaml 
fun last [] = raise Match
  | last [x] = x
  | last (x::xs) = last xs;
```

### 3.3 Take and drop 

```ocaml
fun take ([], _) = []
  | take (x::xs, n) = if n > 0 then x :: take (xs, n-1) 
                               else [];

fun drop ([], _) = []
  | drop (x::xs, n) = if n > 0 then drop (xs, n-1)
                               else (x::xs);
```

### 3.4 nth element of list 

```ocaml 
fun nth ([], _) = raise Match
  | nth (x::xs, n) = if n > 0 then nth (xs, n-1) else x;
```

Official solution uses previously defined `hd` and `drop`.

```ocaml
fun nth(l,n) = hd(drop(l,n));
```

### 3.5 Append lists

```ocaml
fun append (xs, []) = xs
  | append ([], ys) = ys
  | append(x::xs, ys) = x :: append(xs,ys);  
```

Official solution defines an infix function:

```ocaml
infix @;
fun   xs    @ [] = xs
  |   []    @ ys = ys
  | (x::xs) @ ys = x :: (xs@ys);
```

### 3.7 Efficient list reversal

```ocaml
fun rev (x::xs) =
    let fun revAppend ([], acc) = acc
          | revAppend (x::xs, acc) = revAppend (xs, x::acc)
    in revAppend (x::xs, []) end;
```

### 3.9 Zip function that does not depend on order of pattern matching

```ocaml 
fun zip ([], _) = []
  | zip (_, []) = []
  | zip (x::xs, y::ys) = (x, y) :: zip(xs,ys);

fun unzip [] = ([], [])
  | unzip ((x, y)::pairs) = 
        let val (xs, ys) = unzip pairs
        in (x::xs, y::ys) end;
```

### 3.11 Roman numerals

Write a function to convert an integer into Roman numerals, in the expanded form (e.g XIIII) and the condensed form (XIV).  

I was able to do the former as follows:

```ocaml 
fun numerals k = 
    let fun rn ([], _) = []
          | rn ((l, v)::pairs, 0) = []
          | rn ((l,v)::pairs, k) = 
              if k < v then rn (pairs, k) 
              else l :: rn((l,v)::pairs, k-v)
        val letters = [(#"M", 1000), (#"D", 500), 
                       (#"C", 100), (#"L", 50), 
                       (#"X", 10), (#"V", 5), 
                       (#"I", 1)]                     
    in implode(rn (letters, k))
    end;
```

The official solution uses the same algorithm but with string concatenation instead of imploding. I did not realise that representing them in the proper form is just a matter of using a different dictionary

```ocaml 
fun roman (numpairs, 0) = ""
  | roman ((s,v)::numpairs, amount) =
      if amount<v then roman(numpairs, amount)
      else s ^ roman((s,v)::numpairs, amount-v);
      
val rompairs1 =
  [("M",1000), ("D",500), ("C",100), ("L",50), 
   ("X",10), ("V",5), ("I",1)]
   
rompairs2 =
 [("M",1000), ("CM",900), ("D",500), ("CD",400), 
  ("C",100),  ("XC",90),  ("L",50),  ("XL",40), 
  ("X",10),   ("IX",9),   ("V",5),   ("IV",4), 
  ("I",1)];
```

### 3.13 Making change with a finite purse

Write a function to generate all ways of making change given a finite number of each coin denomination in the till.

```ocaml
fun allChange (coins, till, 0) = [coins]
  | allChange (coins, [], amt) = []
  | allChange (coins, (c, 0)::till, amt) = 
        allChange (coins, till, amt)  
  | allChange (coins, (c, n)::till, amt) = 
        if amt < 0 then []
        else allChange(c::coins, (c, n-1)::till, amt - c) @         
             allChange(coins, till, amt);
```

### 3.14 Making change with an accumulator

Rather than using an append function, it would be more efficient to have an accumulator argument. This solution uses `chg` to represent one way of making change, and `chgs` to accumulate all possible ways of making change:

```ocaml 
fun change (till, 0, chg, chgs) = (chg::chgs)
  | change ([], _, chg, chgs) = chgs
  | change (c::till, amt, chg, chgs) =  
        if amt < 0 then chgs 
        else change(c::till, amt-c, c::chg, 
                    change(till, amt, chg, chgs))

fun allChange (till, amt) = change(till, amt, [], []);
```

### 3.15 Binary sum and product for list of booleans.

Using a list of booleans to represent a reversed binary number (e.g `[false, false, false, true]` => 1000), compute the sum and product of a binary number. 


```ocaml 
fun bincarry (false, ps) = ps
  | bincarry (true, []) = [true]
  | bincarry (true, p::ps) = (not p) :: bincarry(p, ps);

fun binsum (c, [], qs) = bincarry(c, qs)
  | binsum (c, ps, []) = bincarry(c, ps)
  | binsum (c, p::ps, q::qs) = 
      (* c p q all false *)
      if not (c orelse p orelse q) then false::binsum(false, ps, qs)
      (* c p q all true *)
      else if (c andalso p andalso q) then true::binsum(true, ps, qs)
      (* exactly one of c p q true *)
      else if (c andalso not p andalso not q) 
              orelse (p andalso not c andalso not q) 
              orelse (q andalso not c andalso not p)
           then true::binsum(false, ps, qs)
      (* exactly two of c p q true *)
      else false::binsum(true, ps, qs);


fun binprod ([], _) = []
| binprod (false::ps, qs) = false::binprod(ps, qs)
| binprod (true::ps, qs) = 
        binsum(false, qs, false::binprod(ps, qs));
```

The solution uses the same overall strategy, but with much slicker boolean algebra for the sum:

```ocaml
fun carry3(a,b,c) = (a andalso b)
                    orelse (a andalso c) 
                    orelse (b andalso c);

(*Binary sum: since b=c computes not(b XOR c), the result is a XOR b XOR c*)
fun sum3(a,b,c) = (a=(b=c));

fun bsum (c, [], qs) = bincarry (c, qs)
  | bsum (c, ps, []) = bincarry (c, ps)
  | bsum (c, p::ps, q::qs) =
      sum3(c,p,q)  ::  bsum(carry3(c,p,q), ps, qs);
```

### 3.18 Converting decimal to binary and a large factorial

Treating decimal numbers as a reversed list of 0-9, define functions that convert between binary and decimal, and define a function to find the factorial of large numbers. 

```ocaml
fun sum [] = 0
  | sum (x::xs) = x + sum(xs);

fun pow x 0  = 1
  | pow x n = if n mod 2 = 0
              then pow (x*x) (n div 2) 
              else x * pow (x*x) (n div 2);

fun binToInt ([], counter) = 0
  | binToInt (p::ps, counter) = 
        (pow 2 counter) * p + binToInt(ps, counter+1);

fun intToDec i = if i < 10 
                then [i] 
                else (i mod 10)::intToDec(i div 10);
fun intToBin i = if i < 2 
                then [i] 
                else (i mod 2)::intToBin(i div 2);

fun decToInt ([], counter) = 0
  | decToInt (d::ds, counter) = 
            (pow 10 counter) * d + decToInt(ds, counter+1);

fun decrement [1] = []
  | decrement (p::ps) = if p = 1 then 0::ps 
                        else 1::decrement ps;

fun fact d = 
  let fun binfact [1] = [1]
        | binfact p = binprod(p, binfact (decrement p))
  in binfact (intToBin d) 
  end;
```

This solution fits the spec up until the factorial. Because I do not have a function to *directly* convert from binary to decimal (I do it via an int), I cannot express the large factorial in decimal even though the value has been computed (in binary). I have not yet figured out how the `decimal_of_binary` function in the official solution works 

```ocaml
fun binary_of_int 0 = []
  | binary_of_int n = (n mod 2) :: binary_of_int (n div 2);

val ten = binary_of_int 10;

fun binary_of_decimal [] = []
  | binary_of_decimal(d::ds) = 
        binsum(0,  binary_of_int d,  binprod(ten, binary_of_decimal ds));

fun double (0,[]) = []
  | double (c,[]) = [c]
  | double (c,d::ds) = 
      let val next = c + 2*d
      in (next mod 10) :: double(next div 10, ds)  end;

fun decimal_of_binary [] = []
  | decimal_of_binary (p::ps) = double(p, decimal_of_binary ps);


fun binfact n =
    if n=0 then [1]  else  binprod(binary_of_int n, binfact(n-1));

rev (decimal_of_binary (binfact 100));
```

