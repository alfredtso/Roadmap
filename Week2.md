# Week2
- [Algorithms](#algorithms)
# Algorithms
- Integer Multiplication
  - Can have different way to do other than third grade way
  - Karatsuba Multiplication
    - Using recursive call 
## Divide and Conquer Paradigm
- 1. Divide into smaller problems
- 2. Conquer via recursive calls
- 3. Combine sol of subproblem into one for the original problem
### Merge Sort
- Pseudocode
  - recursively sort 1st half of the input array
  - recursively sort 2nd half of the input array
  - merge two sorted sublists into one
  - ignore base casese
- Pseudocode for mergeing:
  - for k in range(n)
  - if A(i) < B(j), C(k) = A(i); i++;  else if A(i) > B(j); C(k) = B(j); j++
  - ignore base cases
- Running time of MergeSort
  - initialize i and j has 2 ops
  - if statemention one and assignment and increment plus for loop one; total 4
  - Upshot: RT on array with m numbers is <= 4m + 2 
  - total log2n + 1 lvs, at each lv, have 2^j sub-prob and each has n/2^j size. By above, we can conclude each lv we have 6n, so total 6nlogn + 6n
#### Guiding Principle
- 1. Worst-case analysis
  - holds for every input of length n, good for "general-purpose" rountines
  - As opposed to
    - Average-case analysis:
	- Benchmarks: has 10 benchmark inputs
	- both require domain knowledges
- 2. Wont pay much attentino to constant factors, lower-order terms
  - way easier mathematically
  - constants depends on archi/compiler/languages
  - lose very little predictive power
- 3. Asymptotic analysis:
  - focus on runnig time for large input sizes n
## Asymptotic Analysis
Motivation
- Vocabulary: "big-oh"
- Coarse enough to surpress archi/lang etc details
- sharp enough to make comparisons between different algorithms, esp on large inputs
High-level idea
- suppresee constant factors and lower-order terms(irrelevant for large inputs)
  - Example: nlogn + 6n is just nlogn
- Problem
  - for loop: O(n)
### Big-Oh
- Let T(n) = function on n
- T(n) = O(f(n))?
  - if eventually for all sufficiently large n, T(n) is bounded on multiple of f(n)
### Omega
- "Lower bound"
- T(n) = Omega(f(n)) iff there exist some constants c and n0 s.t T(n) >= c * f(n) for all n >= n0
### Theta 
- between Big-oh and Omega
### Count Inversion in an array
- Divide and conquer
- Key Idea
  - have recursive calls both count inversions and sort
    - Merge subroutine naturally uncover split inversions
- Observation
  - if there is no split inversion, all elements in B are lt in C
  - so the merge step is copy B and concat to C
  - meaning copying from C imply there is split inversion
  - for example if there is 3 elements left in the left array, copying from right array means there is 3 split inversion
- General claim:
The split inversions involving an element y of the 2nd array C are precisely the number left in the 1st array B when y is copied to the output D
### Strassen Matrix Multi
- Matrix recursive multiplication also take cubic time, it takes 8 recursive call each time
- Strassens' Algorithm
  - 1. recursively compute only 7 (cleverly chosen) products
  - 2. do the necessary (clever) addtions + subtractions (still O(n^2) time)
  - 3. better than cubic time
- Details
  - Seven products
    - A(F-H), (A+B)H, (C+D)E, D(G-E), (A+D)(E+H), (B-D)(G+H), (A-C)(E+F)
  - see the rest in notes or google
### Closet 2 Pair
- Input: a set p = {p1, ... ,pn} of n points in plane R2
- use Euclidean distance
- Output: a pair of points in p such that d(p,q) is min for all in p
- Assumption: all distinct points
- can we piggyback mergesort like inversion
- 1-D version of closet pair:
  - 1. sort points
  - 2. return closet pair of adjacent points
#### High Level Approach
- make copies of pointed sorted by x and y (O(nlogn)time)
  - but simple example would tell you its not enough 
Px meaning P being sorted by x-coordinate
- 1. Divide
  - Let Q = Left half of P, R = right half of P, form QxQyRxRy (O(n)time)
- (p1,q1)=CP(Qx,Qy)
- (p2,q2)=CP(Rx,Ry)
- The above two case doesnt cover all cases i.e the split case
- (p3,q3)=CSP(Px,Py)
