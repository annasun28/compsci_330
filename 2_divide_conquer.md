# Insertion Sort
**Insert** takes $O(n)$ time for each number in the array;
we must repeat **insert** $n$ times, so recurrence relation is:

$$ T(n) = T(n-1) + O(n) $$
$$ T(n) = O(n) + O(n-1) + O(n-1) + ... + O(1) = O(n^2)$$

# QuickSort
```python
def pivot(A, l, r):
  r += 1  # why this?
  x = A[l]
  while l < r:
    while A[l] > x:
      l += 1
    while A[r] < x:
      r -= 1
    A[l],A[r] = A[r],A[l]  # swap
  return r

def QuickSort(A, l, r):
  m = pivot(A, l, r)
  A[l],A[m] = A[m],A[l]  # swap
  QuickSort(A, l, m-1)
  QuickSort(A, m+1, r)
```

### Best case recurrence relation:
$$ T(n) = 2T(\frac{n}{2}) + O(n) = O(n \log n) $$

# MergeSort
```python
def MergeSort(A):
  if len(A) <= 1:
    return A
  # split A into B and C
  MergeSort(B)
  MergeSort(C)
  merge(B, C)
```

### Recurrence relation:
$$ T(1) = 0 $$
$$ T(n) \leq 2T(\frac{n}{2}) + O(n) = O(n \log_2 n) $$

### Theorem:
$$ T(n) \leq O(n \log_2 n) $$

### Proof:
1.  When $n=1$:
    $$T(n)=0; O(n \log_2 n)=0$$
2.  Assume for all $n=1,2,...,k$:
    $$T(n)\leq2T(\frac{n}{2})+O(n)$$
3.  When $n=k+1$:
    $$ T(k+1) \leq 2T(\frac{k+1}{2})+O(k+1) $$
    $$ T(\frac{k+1}{2}) \leq O(\frac{k+1}{2}) \log_2 \frac{k+1}{2} $$
    $$ T(k+1) \leq 2O(\frac{k+1}{2}) \log_2 \frac{k+1}{2}+O(k+1) $$
    Assumption still holds when $n=k+1$:
    $$ T(k+1) \leq O(k+1) \log_2(k+1) $$

### Recursion Tree
1.  $O(n)$
2.  $2O(\frac{n}{2})$
3.  $4O(\frac{n}{4})$

### Space Complexity
`MergeSort(B)` and `MergeSort(C)` can use the same set memory
$$ S(n) \leq S(\frac{n}{2}) + O(n) $$
$$ S(n) = O(2n) = O(n) $$

# Counting Inversions
Given array `A[1,...,n]`, count number of inversions where an inversion is $(i,j)$ where $1 \leq i < j \leq n$ and $A[i]>A[j]$.

Ex: `[n, n-1, n-2, ..., 1]` has $\frac{n(n-1)}{2}$ inversions

### Naive Implementation
$$ T(n) = 2T(\frac{n}{2}) + O(\frac{n}{2})^2 $$
```python
def count(A):
  if len(A) <= 1:
    return 0
  # split A into B, C
  return count(B) + count(C) + merge(B, C)

def merge(B, C):
  return sum([1 if b > c else 0 for b in B for c in C])
```

### Sort then count
$$ T(n) = 2T(\frac{n}{2}) + O(\frac{n}{2} \log \frac{n}{2}) $$
```python
def countsort(A):
  if len(A) <= 1:
    return 0
  # split A into B, C
  return countsort(B) + countsort(C) + countmerge(B,C)

def countmerge(B, C):
  sort(C)
  count = 0
  for b in B:
    idx = 0
    while C[idx] < b:
      idx += 1
      count += 1
  return count
```

# Multiplying Bit Strings
$$ xy = x_Ly_L + (x_Ly_R)2^\frac{n}{2} + x_Ry_R $$
Recurrence relation:
$$ T(n) = 4T(\frac{n}{2}) + O(n) = O(n^2) $$

We can reduce this into just 3 multiplications:
$$ xy = x_Ly_L2^n + ((x_L + x_R)(y_L + y_R) - x_Ry_R - x_Ly_L) + x_Ry_R $$
Recurrence relation:
$$ T(n) = 3T(\frac{n}{2}) + O(n) = O(n(\frac{3}{2})^{\log_2 n}) = O(3^{\log_2 n}) = O(n^{log_2 3}) $$

# Master Theorem
### General recurrence relation:
$$ T(n) = aT(\frac{n}{b}) + O(n^d) $$
Each level of the computation tree has $a$ branches. We have a total of $\log_b n$ levels.

-  $O(n^d)$
-  $O(a(\frac{n}{b})^d)$
-  $O(a^k(\frac{n}{b^k})^d)$
-  ...
-  $O(a^{\log_b n}(\frac{n}{b^{\log_b n}})^d) = a^{\log_b n} = n^{\log_b a}$

Sum: $O(n^d + a(\frac{n}{b})^d + ... + a^k(\frac{n}{b^k})^d + ... + n^{\log_b a} )$

Three possible situations:

- $d>\log_b a$: $n^d$ term dominates so $T(n) = O(n^d)$
- $d<\log_b a$: $n^{\log_b a}$ dominates so $T(n) = O(n^{\log_b a})$
- $d = \log_b a$: $T(n) = O(\sum_{j=0}^{\log_b n-1} \frac{a^j}{b^{dj}}n^d) = O(n^d \log_b n)$
