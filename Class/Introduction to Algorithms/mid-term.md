# Sort

## Selection sort

- keep selecting the smallest element from the array and swap it to the right index.
- at most n(4n+C+3) operations O(n^2)

## Insertion Sort

- keep insert new element into sorted array
- O(n^2)

## Merge Sort

- divide array half, sort two of them respectfully, and then merge them.
- merge two sorted array: keep getting min of 2 arrays. O(n)
- O(nlogn)

# Data Structures

## Young Tableau (2D sorted array)

An m by n matrix so that each row and column is sorted in nondetrimental order.

- search costs is O(n+m)
- insertion cost is O(n + m)
- build needs O(nlogn)

## Heaps

Array A is a max heap if every element is less or equal to its parent. (complete binary tree)

- `parent(i)` = i / 2
- `leftChild(i)` = 2*i
- `rightChild(i)` = 2*i+1

- construct needs O(n)
- height of v: number of edges on longest simple path from v to its descendant
- `removeMax`: swap first and last element, `n--`, `maxHeapify` first element

### Heap Sort

Keep remove first element from max heap.

- O(nlogn)

# Divide and Conquer

## Quick Sort

Choose a pivot element, put element smaller than pivot to left, the others to right. Sort two of them seprately.

- Usually O(nlogn), worst case is O(n^2)

### Randomized

choose pivot randomly

- a partition is balanced if the pivot has rank in the range [n/4, 3n/4]
  - two partition has size at least 4/n
  - we have high probability because good nodes and bad nodes are equality likely to appear
  - by Chernoff Bound, the above claim fails whih probability at most 1/n^100
- the probability that run slower than O(nlogn) is 0.0001

## Selection Problem

Find the k-th smallest element in array. Run in expected O(n) time.

- Pick a pivot randomly, partition into two subarray.
- If k is smaller than first subarray, recursively with subarray as  new array
- always choosing good pivot has complexity O(n)
- always choosing bad pivot has complexity O(n^2)
- Assume the good pivot reduce the problem size by a factor of 3/4, bad one doesn't reduce the problem size.
  - good nodes is at most 4n/3
  - bad nodes that immediately follow a good node is at most 4n/3 * 1/2
  - bad nodes that immediately follow a bad node which is follow a good node is at most 4n/3 * 1/4
  - Hence, the expected total runtime is at most 4n(1 + 1/2 + 1/4 + 1/8 + ...)/3 = 8n/3.
  - By Markov inequality, we know with probability 1/2, quick select runs in time less than 16n/3.

# Linear Time Sorting

## Comparison-based sorting algorithms

- cannot access the items' value directly, only compare two items and find out which one is bigger.
- argue that all comparison-based sorting algorithms give rise to a decision tree
  - running an algorithm on a particular input corresponds to a particular path though the tree
  - the runtime is omega(length of the path)
  - binary tree with at least n! leaves.
  - shallowest tree has depth `log(n!)`
  - log(n!) is about `nlog(n/e)` = `omega(nlog(n))`

## Counting Sort

- make 10 bucket (queue) labeled 0 ~ 9
- put elements into bucket according to least significant digit
- popping every queue from 0 ~ 9, append to new array
- use new array to evaluate next significant digit
- `O(nd)`, `d` is iteration times

## Radix Sort Running Time

Bigger base means more buckets but fewer iterations.

- `n` integers
- maximum size `M`
- base `r`
- `d = log(M) / log(r) + 1`
- complexity is `O(d * (n + r))`

# Tree

## Binary Search Tree

- every left descendant of a node has key less than that node, vice versa.
- search/insert take O(tree height)
- delete
  - no children: just remove
  - one children: move it up
  - two children: replace it with next larger number (the smallest node that larger than it)

### Self-Balancing Binary Search Tree

- Rotations
  - pick new root of the tree
  - if there are three child, fell down old right child of root under right child's left child.
- have some proxy for balance
  - if the tree satisfies some property, then it's pretty balanced.
  - maintain property using rotations

#### Red-Black Trees

Maintain balance by setting black nodes are balanced, and that there aren't too many red nodes.

Rules:

- Every node is colored red or black.
- The root node is a black node.
- Children of a red node are black nodes.
- For any node, all simple path to its leaves (NIL) have same number of black nodes.

Property:

- height at most `2log(n + 1)`
- all operation with RBTrees are `O(log(n))`

# Asymptotics and Recurrences

## Big-O-notation (upper bounds)

$O(g(n))$ = {$f(n)$: there exist positive constants $c$ and $n_0$ such that $0 \leq f(n) \leq c*g(n)$ for all $n \geq  n_0$}

## Asymptotic Dominance

Let $a$ and $b$ be numeric functions. $a$ is asymptotically dominates $b$ if there exist positive constants $k$ and $m$ such that $|b(r)| \leq m*a(r)$ for $r \geq k$

## $\Omega$-notation (lower bound)

$\Omega(g(n))$ = {$f(n)$: there exist positive constants $c$ and $n_0$ such that $0 \le c*g(n) \le f(n)$ for all $n \le n_0$}

## $\Theta$-notation (tight bound)

$\Theta(g(n))$ = {$f(n)$: there exist positive constants $c_1$, $c_2$, $n_0$ such that $0 \leq c_1*g(n) \leq f(n) \leq c_2*g(n)$ for all $n \geq n_0$}

For any two functions $f(n)$ and $g(n)$, we have $f(n) = \Theta(g(n))$ iff $f(n) = O(g(n))$ and $f(n) = \Omega(g(n))$.

## $o$-notation and $\omega$-notation

change `<=` to `<`

## Substitution Method

1. **Guess** the form of the solution
2. **Verify** by induction
3. **Solve** for constants

## Recursion Tree Method

- A recursion tree models the costs of a recursive algorithm.
- unreliable, good for **generating guesses** for the substitution method

## The Master Method

apply to recurrences of the form $T(n) = aT(n/b) + f(n)$, where $a \geq 1$, $b > 1$, and $f$ is asymptotically positive

- $a$: number of subproblems
- $n/b$: subproblem size
- $f(n)$: work dividing and combining

compare $f(n)$ with $n^{\log_ba}$:

- $f(n) = O(n^{\log_ba - \epsilon})$ for some constant $c > 0$
  - $T(n) = \Theta(n^{\log_ba})$
- $f(n) = \Theta(n^{log_ba}lg^kn)$ for some constant $k \geq 0$
  - $T(n) = \Theta(n^{log_ba}lg^{k+1}n)$
- $f(n) = \Omega(n^{log_ba+\epsilon})$ for some constant $\epsilon > 0$
  - $T(n) = \Theta(f(n))$

## Generating Function

Consider the recurrence relation 
$$
a_r = 3a_{r-1}+2, r\geq1
$$
Multiplying both sides by $z^r$, we obtain
$$
a_rz^r=3a_{r-1}z^r+2z^r
$$
Summing for all $r, r \geq 1$, we obtain
$$
\Sigma^\infin_{r=1}a_rz^r=3\Sigma^\infin_{r=1}a_{r-1}z^r+2\Sigma^\infin_{r=1}z^r
$$
Note that
$$
\Sigma^\infin_{r=1}a_rz^r=A(z)-a_0\\
\Sigma^\infin_{r=1}a_{r-1}z^r=z\Sigma^\infin_{r=1}a_{r-1}z^{r-1} = zA(z)\\
\Sigma^\infin_{r=1}z^r=\frac{z}{1-z}
$$
We obtain
$$
A(z)-a_0=3zA(z)+\frac{2z}{1-z}
$$
That is, 
$$
(1-3z)A(z)=\frac{2z}{1-z}+1
$$
which simplifies to
$$
(1-3z)A(z)=\frac{1+z}{1-z}\\
A(z)=\frac{2}{1-3z}-\frac{1}{1-z}
$$
Consequently, we have
$$
a_r=2*3^{r-1}-1, r\geq0
$$
