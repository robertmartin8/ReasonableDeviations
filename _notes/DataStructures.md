---
layout: default
title: DataStructures
---

# Data Structures

<!-- TOC -->
- [Amortised analysis](#amortised-analysis)
- [Stack ADT](#stack-adt)
- [List ADT](#list-adt)
- [Queue ADT](#queue-adt)
- [Dequeue ADT](#dequeue-adt)
- [Dictionary ADT](#dictionary-adt)
- [Set ADT](#set-adt)
- [Binary Search Tree](#binary-search-tree)
- [2-3-4 tree](#2-3-4-tree)
    - [2-3-4 Insertion](#2-3-4-insertion)
- [Red Black Tree](#red-black-tree)
    - [RBT Insertion](#rbt-insertion)
- [B-tree](#b-tree)
    - [B-tree insertion](#b-tree-insertion)
    - [B-tree deletion](#b-tree-deletion)
- [Hash-tables](#hash-tables)
- [Binary heap](#binary-heap)
- [Binomial heap](#binomial-heap)
- [Fibonacci heap](#fibonacci-heap)

<!-- /TOC -->

## Amortised analysis



## Stack ADT

LIFO data structure that only offers operations for the top item

```java
ADT Stack {
    boolean isEmpty();
    void push(Item x);
    Item pop(); // precondition: !isEmpty
    Item top(); // precondition: !isEmpty
}
```

Commonly implemented using an array and a TOS (top-of-stack) index


## List ADT



## Queue ADT

Similar to a stack, but is LIFO.

```java
ADT Queue {
    boolean isEmpty();
    void push(Item x);
    Item pop(); // precondition: !isEmpty
    Item peek(); // precondition: !isEmpty
}
```

A double-ended queue (**deque**) generalises the stack/queue to a data structure that allows insertions and extractions at the front or rear. 

## Dictionary ADT

```java
ADT Dictionary {
    void set(Key k, Value v);
    Value get(Key k);
    void delete(Key k);
}
```

If they keys are drawn from a relatively small range of integers, we can use **direct addressing** where the keys are just indices into an array, in which the values would be stored. 

However, dictionaries are most often implemented as [binary search trees](#binary-search-tree).

## Priority queue ADT

- Used to keep track of a dynamic set of objects whose keys support a total ordering. 
- Like a FIFO queue, except each key corresponds to a priority – items with higher priority move to the top.

```java
ADT PriorityQueue {
    void insert(Item x)
    Item first();
    Item popmin();
    void decrease_Key();
    void delete(Item x);
}
```

## Binary Search Tree

[*Interactive practice*](https://www.cs.usfca.edu/~galles/visualization/BST.html)

- Binary search trees (BSTs) are made of nodes that point to two sub-trees. The left sub-tree will be items smaller than the key of the node, right sub-tree will be larger items. 
- The **successor** will be the left-most node in the right subtree (if that exists). Otherwise, walk up the tree until the link goes up-and-right. 
- If a node has two children, its successor has no left child (otherwise there would be a smaller node).
- Most operations are $O(\lg n)$ if the tree is balanced, but balance cannot be guaranteed.

### BST Deletion 

- It is trivial to delete a leaf node or a node with one child
- To delete a node with two children, replace it with its successor (which must come from the right subtree), then delete, because the successor has no left child.


## 2-3-4 tree 

Each node can now have 2, 3, or 4 children (i.e 1, 2, 3 keys).

### 2-3-4 Insertion 

Always try to insert into the bottom layer: if you see any 4-nodes along the way, split them into two 2-nodes. Thus the tree can only increase in height if the root is a 4-node, in which case it is split into three 2-nodes.

1. Go to the bottom level in the tree to where the key should be added.
2. If the place is a 2-node or 3-node, insert directly.
3. Otherwise, if we are going to insert into a 4-node, split it into two 2-nodes, and move the middle key up to the current level. 

## Red Black Tree

[*Interactive practice*](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)

An RBT is a BST that satisfies five invariants:

1. Every node is either red or black
2. The root is black 
3. All leaf nodes are black, and don't contain data
4. If a node is red, its children are both black.
5. **Any path from a given node to leaves contains the same number of black nodes.**

Red black trees are isomorphic to 2-3-4 trees, but has the advantage that it is easier to code:

<center>
<img src="{{ site.imageurl }}/note_img/234_isomporphism.jpg" style="width:50%;"/>
</center>

Operations that don't modify the tree structure are the same as for BSTs. But in order to preserve the RBT invariants, other operations require **rotations**. 

<center>
<img src="{{ site.imageurl }}/note_img/bst_rotation.png" style="width:80%;"/>
</center>

In the diagram above, D has been rotated to the right, then B has been rotated to the left. These rotations apply to non-root nodes as well.

To rotate a parent *x* right given its left child *y*:

```python
if y != null:
    x.l = y.r
    y.r = x
    x = y
```

Likewise, a left-rotation:

```python
if y != null:
    x.r = y.l
    y.l = x
    x = y
```

### RBT Insertion 

Let *p* denote the parent, *g* the grandparent, *u* the uncle, and *n* the new node to be inserted.

If *p* is black, we can just insert a red child directly. 

If *p* is red, we will insert a red *n* then clean up the tree. In this case, *g* must be black (because the tree cannot have two red nodes in a row).

**Case 1 – red uncle**

<center>
<img src="{{ site.imageurl }}/note_img/rbt_case1.png" style="width:50%;"/>
</center>

- Easy: flip colour of *p*, *u*, *g*.
- If *g* is a root, just recolour it black and finish.
- If *g* has a red parent, we have moved the problem up two levels.

**Case 2 – black uncle, bent**

<center>
<img src="{{ site.imageurl }}/note_img/rbt_case2.png" style="width:50%;"/>
</center>

- Left-rotate *n* and we are now in case 3.

**Case 3 – black uncle, straight**

- Swap colours of *p* and *g*, then right-rotate *g*.

<center>
<img src="{{ site.imageurl }}/note_img/rbt_case3.png" style="width:70%;"/>
</center>


## B-tree

B-trees are a generalisation of BSTs with a high branching factor, with the idea that children may actually be stored on a separate disc.

Each node of a B-tree has a lower and upper bound on the number of keys it may contain (except the root has no lower bound). 

A B-tree is parameterised by a **minimum degreee** *t* such that each node will have between *t* and *2t* pointers. A 2-3-4 tree is thus a B-tree of degree 2.

### B-tree insertion

- On the way down, whenever you find a full node, split it into two and move the median key up one level.
- Insertion can only happen in the bottom level.

### B-tree deletion 

- We can only delete a key from the bottom otherwise its children lose their separator. Hence we start by replacing the key with its successor.
- But we must prepare the tree first, because deleting might violate the minimum fullness requirement.
- Hence if deletion would cause the node to become too small, we **refill** a node, redistributing some keys from its siblings if they can afford to lose some, or merging. 
- Merging siblings makes the parent lose a key, so we might have to recursively refill the parent. 

## Hash-tables

- A hash-table is a data structure that may be used to implement the dictionary ADT. 
- Keys are mapped to an integer between 0 and $m-1$ with some **hash function** $h(k)$, and can then be stored at that index in an array of size *m*.
- If two keys map to the same location, there is a **hash collision**

### Chaining

- Each array location points to a linked list. 
- With a good hash function, keys will be evenly distributed in the array. 
- Worst case is $O(n)$ if all items hash to one bucket.


### Open addressing

- $h(k)$ is the first preference for where to store a given key. If it is already full, we use some rule to **probe** the hash table for another position.
- - We then follow this succession rule every time we query an item, checking for key equality. 
- When an item is deleted, it should be marked as deleted rather than just removed – otherwise it interferes with the probing. 
- - If insertion causes the occupancy to increase beyond a certain threshold, the size of the hash table should be increased (like with dynamic arrays).

- **Linear probing** simply finds the next available memory cell and inserts it there. 
    - return $h(k) + j \mod m$, where *j* is the number of attempts
    - leads to **primary clustering**, in which failed attempts hit the same bucket and overflow to the same buckets, resulting in long runs of occupied buckets
- **Quadratic probing** is an improvement that increases the space between guesses:
    - return $(h(k) + cj + dj^2) \mod m$
    - doesn't suffer from primary clustering, but suffers from **secondary clustering** in which two keys that hash to the same value lead to the same probing sequence.
- **Double hashing** suffers from neither primary nor secondary clustering:
    - return $h_1(k) + j h_2(k) \mod m$ using different hash functions.
    - thus keys that hash to the same value will have different probing sequences
    - best open-addressing scheme in terms of collision reduction but has overhead of extra hash


## Binary heap (TBC)

- A heap is an **almost full binary tree** which satisfies the **heap property**, where each node has a value at least as large as those of its children. Thus the root node is the maximum element.
- Isomorphic to an array where `a[k] >= a[2k+1]` and `a[k] >= a[2k+2]`.
- Min-heaps can be used to implement the priority queue ADT, allowing for easy access of the highest priority item.

## Binomial heap

- One problem with binary heaps is that they have an $O(n_1 + n_2)$ cost to merge (concatenate arrays then heapify).
- A binomial tree of order 0 is a single node (height 0).
- A binomial tree of order *k* is obtained by combining two binomial trees of order $k-1$.
    - contains $2^k$ nodes
    - height is always equal to order

<center>
<img src="{{ site.imageurl }}note_img/binomial_heap.png" style="width:80%;"/>
</center>

- A binomial heap is a forest of **binomial trees**, at most one of each order.
    - if a heap contains *n* nodes, it contains $O(\lg n)$ binomial trees, the largest of which has order $O(\lg n)$.
    - can be thought of as a binary number
- `first` is $O(\lg n)$ because we have to scan through the root of each binomial tree in the heap.
- Merging is very similar to binary addition, and is thus $O(\lg (n_1 + n_2))$.
- `popmin` requires $O(\lg n)$ to find the minimum. The children of this minimum are another binomial heap, so after deleting the minimum we can merge it with the rest in $O(\lg n)$.
- `push` can be viewed as merging a new binomial heap which only contains a tree of order 0. It is $O(1)$ amortised because most operations don't touch many trees, but $O(\lg n)$ worst case.

## Fibonacci heap

- 