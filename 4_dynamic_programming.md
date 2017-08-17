# Fibbonacci numbers
`F(n) = F(n-1) + F(n-2)`

```python
def fib(n):
  if n<=1:
    return 1
  return fib(n-1) + fib(n-2)
```
Runtime: $T(n) = T(n-1) + T(n-2) + O(1)$

### Memoization
```python
def fib(n):
  if n<=1:
    return 1
  sols = [None]*n+1
  f[0] = 1
  f[1] = 1
  for i in range(2,n+1):
    f[i] = f[i-1] + f[i-2]
  return f[n]
```

# General Idea of DP
Save intermediate results to avoid repeated computation

# Design a DP Algorithm
1.  Identify important subproblems (make a table, list, matrix...)
2.  Fill in the entries of the table in a "good" order

# Shortest Path in DAG
Given a DAG, edge $(i,j)$ has length $W_{i,j}$. We want to find the shortest path from $s$ to $t$.

### Recursive Solution
What is the last step of the solution?
```python
def shortest(v, t):
  if v = t:
    return 0
  min_path = infinity
  for u in v.children:
    min_subpath = shortest(u, t)  # this could be memoized
    if min_subpath + weight(u,v) < min_path:
      min_path = min_subpath + weight(u,v)
  return min_path
```
Recurrence relation:
$$ T(n) = \sum_{i<n} T(i) + O(n) = O(n^n) $$

### Memoization
```python
  dist = {}
  def shortest(v, t):
    if v == t:
      return 0
    global dist
    if dist[v] is not None:
      return dist[v]
    min_path = infinity
    for u in v.children:
      min_subpath = shortest(u, t)
      if min_subpath + weight(u,v) < min_path:
        min_path = min_subpath + weight(u,v)
    dist[v] = min_path
    return min_path
```

# Longest Common Subsequence
Input: 2 sequences $a = 1,2,3,2,1$ and $b=2,3,1,4,1$

A subsequence is a subset of elements in the same order (not necessarily continuous)

Output: length of the longest common subsequence of $a$ and $b$ - $2,3,1$

Do `a[-1]` and `b[-1]` belong in the LCS? 3 cases:
1.  `a[-1] == b[-1]` - possibly both in LCS
    1.  `LCS = LCS(a[:-1], b[:-1]) + a[-1]`
2.  `a[-1]` not in LCS
    2.  `LCS = LCS(a[:-1], b)`
3.  `b[-1]` not in LCS
    3.  `LCS = LCS(a, b[:-1])`

Subproblem: LCS for the first $i$ elements of $a$, and first $j$ elements of $b$

1.  Define a table to store the result of subproblems
    - `f[i][j]` = length of LCS for first $i$ elements of $a$ and first $j$ elements of $b$
2.  Write recursive formula: assume we already know `f[i-1][j-1]`, `f[i-1][j]`, and `f[i][j-1]`
    ```python
    if a[i] == b[j]:  
      f[i][j] = max(f[i-1][j-1]+1,
                    f[i-1][j],
                    f[i][j-1])
    ```
3.  Boundary condition `f[0][j] = 0` and `f[i][0] = 0` $\forall i,j$
```python
def lcs(a,b):
  # initialize all cases where subproblem len(a) or len(b) = 0
  f = [[0] + [None]*(len(b)-1) for i in range(len(a))]
  f[0] = [0] * len(b)

  for i in range(1,len(a)):
    for j in range(1,len(b)):
      f[i][j] = max(f[i-1][j], f[i][j-1])  # cases 2 and 3
      if a[i] == b[i] and (f[i-1][j-1]+1) > f[i][j]:  # case 1
        f[i][j] = f[i-1][j-1]+1
```
[
[0,   0,    0,    0],
[0,   0, None, None],
[0,   None, None, None],
[0,   None, None, None]]
```
