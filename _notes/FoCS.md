---
layout: default
title: Foundations of Computer Science
---

# Foundations of Computer Science

- Computing is largely about abstraction: what services should be provided at different levels and how should these interface?
- The goal of programming is to describe computation mechanically, allowing for correct, efficient and maintainable code. 
- ML abstracts away the underlying hardware (and memory management), and is similar to mathematics.

## Basic syntax

This is a **value declaration** with `pi` as the identifier. 

```ocaml
val pi = 3.1415;
```

If we compute an expression, ML actually declares an identifier `it` and sets it to the value of the computed expression. We can also perform a simultaneous declaration using `val a=1 and b=2` (which can also swap names).


Functions are also values, just with a special declaration syntax. This invites the use of recursion:

```ocaml
fun npower (x, n): real = 
    if n = 0
    then 1.0
    else x * npower (x, n-1);
> val npower = fn : real * int -> real
```

```ocaml
fun gcd (m,n) =
    if m = 0 then n else gcd (n mod m, m);
```

All ML functions only operate on one argument. The type signature `real * int` specifies that this argument is a `(real, int)` tuple.

Operators are overloaded between different types: e.g multiplication is a different operator for reals and integers but in both cases it uses `*`. Some operators are different, for example `/` is not defined for integers (`div` must be used instead). All the operators (infix or not) are treated as functions, and therefore as values.

```ocaml
~;
> val it = fn: int -> int
```

Infix operators can be defined:

```ocaml
infix ||;
fun x || y = x*x + y*y;
```

Although ML usually infers types, in ambiguous cases it is best to provide a type annotation as follows:

```ocaml
fun square (x: real) = x * x
```

In ML, `=` is both used for assignment and equality. In general we should only do equality testing if the arguments are of the same type. Inequality is tested for using `<>`. 

Conditional expressions take the form:

```ocaml
if p then E1 else E2
```

p is a **predicate** - a boolean expression that determines which of E1 or E2 is evaluated. The boolean operators are `not`, `andalso`, `orelse`. The latter two are not functions because they only evaluate the second expression if needed. They are just shorthand, e.g `p andalso q` $\equiv$ `if p then q else false`.

e.g efficiently raising a number to a power:

```ocaml
fun power(x:real, n) = 
    if n=1 then x
    else 
        if (n mod 2 = 0) then power(x*x, n div 2)
        else x * power(x*x, n div 2);
```

ML determines the type of an expression without computing the expression


## Expressions and complexity

Expression evaluation is a model for computation in which an expression $E_i$ reduces to an expression $E_{i+1}$ until this process concludes with value *v*. We write $E_i \implies E_{i+1}$ rather than $E_i = E_{i+1}$ because computation is typically one-way. Functional programming deals with expressions and resulting values rather than states.

ML's evaluation rule is **call-by-value** (or **strict** evaluation), rather than call-by-need (lazy) evaluation. To compute the value of $f(E)$, it first computes the value of $E$. 

It is possible to have iteration in a functional paradigm using **tail recursion**. This is done by adding an **accumulator** argument which keeps track of some total. Compared to the recursive approach, iterative solutions tend to require much less space. However, the additional parameters can sometimes reduce clarity.

```ocaml
fun iterateFactorial (n, total) = 
    if n = 0 then total else iterateFactorial (n - 1, n * total);
```

We can attempt to analyse the quality of algorithms by considering their time and space complexity as a function of input size *n*. This is usually considered asymptotically using big O notation. We say that:

$$f(n) \subseteq O(g(n)) \qquad \text{if} \qquad |f(n)| \leq c | g(n)|$$

Similarly, we can define $\Omega(g(n))$ if *g* is the lower bound and $\Theta(g(n))$ if *g* is both the lower and upper bound.

$$O(1) \subseteq O(\log n) \subseteq O(n) \subseteq O(n \log n) \subseteq O(n^2) \subseteq O(a^n)$$

Time complexity must be greater than or equal to space complexity, because the space must be traversed.

Tail recursive algorithms often have constant space complexity because they only need to store one value.

We can find the big O complexity by finding a recurrence relation on the cost function $T(n)$, assuming a base case of $T(0) = 1$ or $T(1) = 1$.


Recurrence  |  Big O complexity
--|--
 $T(n+1) = T(n) + 1$ |  $O(n)$
 $T(n+1) = T(n) + n$ |  $O(n^2)$ 
 $T(n+1) = 2T(n/2) + 1$ |  $O(\log n)$ 
 $T(n+1) = 2T(n/2) + n$ |  $O(n \log n)$  |  

## Lists

A list is a finite ordered series of elements of the same type. It is built from two primitives:

- `[]` (or `nil`) is the empty list. This is polymorphic and has type `'a list`.
- `::` is the cons operator, such that `x::l` is the list with head `x` and tail `l`. 
- Cons is an $O(1)$ operation because under the hood ML is using linked lists.

### Strings

Strings are not treated purely as lists of characters. There are special methods for strings:

- `explode` converts a string into a list of chars
- `implode` does the opposite
- `size` finds the number of chars in a string
- `a ^ b` for concatenation.


### List functions

There are two useful built-in functions:

- The infix append operator `@`. The operation `xs @ ys` is $O(xs)$: in general it is much better to use cons directly. 
- The function `rev` to reverse a list in $O(n)$.

We can define functions with separate clauses in order to pattern-match. 

```ocaml
fun nlength [] = 0
    | nlength (X::xs) = 1 + nlength xs;
```

For list functions, sometimes using cons instead of append leaves the result the wrong way round. The cost of `rev` should be considered.

We can define functions to take or drop the first `k` elements as follows:


```ocaml
fun take ([], _) = []
    | take (x::xs, k) = 
        if (k > 0) then x::take(xs, k-1) 
        else [];
    
fun drop([], _) = []
    | drop (x::xs, k) = 
        if (k > 0) then drop(xs, k-1)
        else (x::xs); 
```

Although these functions have the same complexity, drop will be faster (i.e smaller constant factor) because it is discarding elements.

### Searching 

To find an item in a list, we can loop through items in a linear search with $O(n)$ complexity. If the list is pre-sorted, items can be found in $O(\log n)$. If the list is indexed (as in a hash table), lookup is $O(1)$.

```ocaml
fun member (x, []) = false
    | member (x, y::ys) = 
            (x=y) orelse member (x, ys);
> val member = fn: ''a * ''a list -> bool
```

In the type signature, `''a` means that the function only accepts **equality types**. This includes most types except:

- reals (because of floating point issues)
- functions, because there is no way of knowing apriori whether the output would be the same for all possible input.
- abstract types

### Zipping and Unzipping

We desire:

```ocaml
zip: ('a list * 'b list) -> ('a * 'b) list
unzip: ('a * 'b) list -> ('a list * 'b list)
```

Because patterns are tested in order, we can write a zip function as follows:

```ocaml
fun zip (x::xs, y::ys) = (x, y) :: zip(xs, ys)
    | zip _            = []
```

If there is a case where one of the lists is empty, the first pattern will not match so the second will just produce an empty list.

The unzip function is best written with a local declaration:

```ocaml
fun unzip [] = ([], [])
    | unzip ((x,y)::pairs) =
        let val (xs, ys) = unzip pairs
        in (x::xs, y::ys)
        end;
```

It could be more obviously written with accumulator arguments, but we will then have to reverse it. 

### Making change

The simplest greedy algorithm is straightforward to implement, noting that:

- Change for zero consists of no comparisons
- Otherwise we try the largest available and reduce the outstanding coin. Discard if the coin is too large.

```ocaml
fun change (till, 0) = []
    | change (c::till, amt) =
        if amt<c then change(till, amt)
        else c::change(c::till, amt-c);
```

However, this greedy algorithm can fail in simple cases. We can define an exhaustive iterative solution as follows, where `chg` is the current solution and `chgs` is the list of solutions:

```ocaml
fun change (till, 0, chg, chgs) = (chg::chgs)
    | change ([], _, chg, chgs) = chgs 
    | change (c::till, amt, chg, chgs) =
        let 
            val otherSol = change(till, amt, chg, chgs)
        in 
            if amt<0 then chgs
            else change(c::till, amt-c, c::chg, otherSol) 
        end;
```

- In the base case, we cons the current `chg` to a list of all possible `chgs` and return it.
- At any point, if `amt` is zero, we stop and return `chgs`.
- Otherwise, we deduct the coin at the top of the till from `amt` and continue (same as in greedy). At the same time we recursively explore other possibilities (e.g ignore the biggest coin even if we can use it).

### Defining a set

We can emulate functioning of a set using lists. We want:

- Membership testing 
- Adding an item
- Converting a list to a set
- Union
- Intersection

```ocaml
infix mem;

fun mem (x, []) = false 
    | mem (x, y::ys) = (x=y) orelse mem (x, ys);
    
fun addmem (x, l) = if mem (x, l) then l else x::l;

fun setof [] = [] 
    | setof (x::xs) = addmem(x, setof xs);
    
fun union ([], l) = l
    | union (x::xs, l) = union(xs, addmem(x, l));
    
fun intersect ([], l) = []
    | intersect (x::xs, l) = 
            if (x mem l) then x::intersect(xs,l) 
            else intersect(xs, l);
```



## Sorting

Sorting is important because searching a sorted collection only requires $O(\log n)$ time (via a binary search) compared to $O(n)$ time for a linear search. 

Efficiency of sorting algorithms is analysed by considering the number of comparisons that must be made. The optimal complexity of a comparison-based algorithm must be $O(n \log n)$, which can be seen by noting that in the best case each comparison eliminates half of the permutations.

$$2^{C(n)} \geq n! \implies C(n) \geq \log_2(n!) \approx n \log n - 1.44n$$


### Insertion sort

Works by recursively inserting elements into sorted lists. 

```ocaml
fun ins(x, []) = [x]
    | ins(x, y::ys) = 
        if (x <= y) then x::y::ys 
                    else y::ins(x, ys);
                    
fun insort [] = []
    | insort (x::xs) = ins(x, insort xs);
```

- `ins` inserts an element into a sorted list, and requires $n/2$ comparisons on average (worst case *n*).
- `insort` calls `ins` on average *n* times, so the overall algorithm is $O(n^2)$.

### Quicksort

Quicksort is a divide and conquer algorithm:

- Choose a pivot element *a*
- Partition the list into one with elements greater than *a* and the other with elements ≤ a (this is an $O(n)$ operation).
- Sort sublists, then append together. 


```ocaml
fun quick [] = []
  | quick [x] = [x]
  | quick (pivot::xs) =  
      let fun part (l, r, []) = (quick l) @ (pivot::quick r)
            | part (l, r, y::ys) = 
                    if y <= pivot 
                    then part (y::l, r, ys)
                    else part (l, y::r, ys)
      in part ([], [], xs) end;
```


In the average case, quicksort is $O(n \log n)$. If the pivot divides the input into two equal parts at every step, we have 

$$T(n) = n + 2T(n/2) = O(n \log n)$$

In the worst case, all but one of the items end up in one partition so we have:

$$T(n+1) = n + T(n)  = O(n^2)$$

In both cases, the $n$ comes from the partition (and the append in ML). An iterative version would have the same complexity but would have $n$ rather than $2n$ since it doesn't append.

### Mergesort

The `merge` function merges two sorted lists (but doesn't work well for arrays), requiring at most $m+n$ comparisons.

```ocaml
fun merge ([], ys) = ys
  | merge (xs, []) = xs
  | merge (x::xs, y::ys) = 
        if x <= y then x :: merge(xs, y::ys)
        else y :: merge(x::xs, ys);
```

Mergesort splits a list in two, recursively sorts sublists, then combines them using `merge`.

```ocaml
fun mergesort [] = []
  | mergesort [x] = [x]
  | mergesort xs = 
      let val k = (length xs) div 2
      in merge (mergesort (take(xs, k)), mergesort(drop(xs, k)))
      end;
```

Because `take` and `drop` divide the list into two equal parts, the complexity is always $O(n \log n)$ because:

$$T(n) = 2T(n/2) + n$$

In practice, quicksort is slightly faster (except for the worst case complexity).

## Datatypes

ML allows us to declare new types using the `datatype` declaration. The identifiers of a new type are called **constructors**:

```ocaml
datatype vehicle = Bike
                |  Car
                |  Lorry  
```

These types are treated as if they were built-in, so we can write functions on datatypes with pattern matching. The previous example is just an **enumeration type**. We can associate data with each constructor as follows:

```ocaml
datatype vehicle = Bike of int (*n wheels*)
                |  Car of bool (*diesel or not*)
```


### Exceptions

Raising an exception abandons the current computation; handing the exception attempts an alternative. 

```ocaml
(* exception Failure *)
exception Failure;
exception HttpError of int;

raise Failure;
raise (HttpError 404);
```

Exception names are constructors of the builtin datatype `exn`, meaning that exception handlers can pattern match. 

```ocaml
E handle Failure => E1 
    | (HttpError 404) => E2;
```

Using exceptions we can write a backtracking algorithm to make change:

```ocaml
fun change (till, 0) = []
  | change ([], amt) = raise ChangeError
  | change (c::till, amt) =
        if amt < 0 then raise ChangeError
        else (c :: change(c::till, amt - c))
        handle ChangeError => change(till, amt);
```

If a particular choice of coin makes it impossible to find change in the next recursive call, the handler undoes the most recent choice. If making change is impossible, the second pattern will eventually match an the ChangeError will be raise with no handler. 

An alternative to exceptions is to declare an `option` datatype, but this requires constant checking for NONE

```ocaml
datatype 'a option = NONE | SOME of 'a;
```


## Binary trees

The binary tree is built on a recursive datatype, wherein each node is either empty (i.e `nil`), or two subtrees.

```ocaml
datatype 'a tree = Lf
                | Br of 'a * 'a tree * 'a tree;
```

Declaring a tree:

```ocaml
val tr = Br(1, Br(2, Br(4, Lf, Lf), Br(5, Lf, Lf)), Br(3, Lf, Lf));
```

Simple tree functions:

```ocaml
fun count Lf = 0
  | count (Br(v, t1, t2)) = 1 + count(t1) + count(t2);
    
fun depth Lf = 0
  | depth (Br(v, t1, t2)) = 1 + Int.max(depth t1, depth t2);
```


### Dictionaries and binary search trees

A dictionary is an abstract type with the following components

- lookup
- update (insert)
- delete
- empty (a null dictionary)
- missing exception

Using a list of (k,v) pairs entails $O(n)$ lookup. A **binary search tree** can be used instead as long as the keys have a total ordering. Each branch of the tree carries a (k,v) pair. The left subtree contains smaller or equal keys; the right subtree contains greater keys. For a balanced tree, update and lookup are both $O(\log n)$.

```ocaml
exception Missing;

fun lookup (Br ((k, v), t1, t2), a) =
    if a < k then lookup (t1, a)
    else if a > k then lookup (t2, a)
    else v;
  | lookup (Lf, a) = raise Missing;

fun update (Lf, k, v) = Br((k,v), Lf, Lf)
  | update (Br((a,b), t1, t2), k, v) = 
        if k < a then Br((a,b), update (t1, k, v), t2)
        else if k > a then Br((a,b), t1, update (t2, k, v))
        else (*overwrite*) Br((a,v), t1, t2);
```

This update function overwrites if the keys are equal. Unchanged parts of the tree are shared (under the hood), so this is efficient. 


### Traversing trees

Trees can be traversed in three ways:

- **preorder** - label, left subtree, right subtree
- **inorder** – left subtree, label, right subtree
- **postorder** - left subtree, right subtree, label. 
- **Breadth-first** (discussed later).

These can be very clearly implemented using `@`, but this has quadratic complexity.

```ocaml
fun inorder Lf = []
  | inorder (Br(v, t1, t2)) = inorder t1 @ [v] @ inorder t2;
```

Instead, the functions should be defined using an accumulator.

```ocaml
fun preorder (Lf, vs) = vs
  | preorder (Br(v, t1, t2), vs) = 
            v :: preorder (t1, preorder (t2, vs));
        
fun inorder (Lf, vs) = vs
  | inorder (Br(v, t1, t2), vs) =
            inorder (t1, v::inorder (t2, vs));
            
fun postorder (Lf, vs) = vs
  | postorder (Br(v, t1, t2), vs) =
            postorder (t1, postorder (t2, v::vs))    
```

