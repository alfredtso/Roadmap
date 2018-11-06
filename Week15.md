# SoftConstruction
### Call Graphs
- A class contains all the behaviours and data for a single concept
- A method call looks like: methodName() or otherMethod(x,y)
- The call graph should only concern with the package within our program, not say Java's
### Class and Objects
- Class
  - include
  - data
  - opeation
  - eg : Dog has age data and barking operation
##### Variables
- final: means immutable
- static: means it belongs to the class instead of a particular instance
#### Debugging
- Hyothesis Driven Debugging
- valid or ruleout
#### Dataflow
- primitive like int the variable store the value
- but reference to instance of class stores the address to that object
# Algorithm
### Randomized Selection 
- Goal: to select the ith statics
- modify QuickSort to do: first choose pivot element and then see which subproblem to recurse
- Running Time
  - Worst in quicksort happens when the first/last element, similar in selection
  - best pivot would be median
- Average running time is O(n)
- Proof
  - Rselect is in phase j if current array size is between (3/4)^j+1n and (3/4)^jn
  - phase quantity we make 75% progress
  - Xj = number of recursive calls during phase j 
  - if Rselect choosese a pivor giving a 25-75 split (or better) then current phase ends
    - probability of this split or better is 50%
	- Xj is the expectation of numbers of time needed to flip you get better getting head
### Deterministic Selection
- Recall: Median is the best pivot
- Goal: Find a pivot guaranteed to be good
- Keyidea: median of medians
  - Break the array A into groups of n/5 size, sort each group
  - copy the middle elements of each array to a new array C
  - p = Dselect(C, n/5, n/10) [recursively compute median of C]
  - Partition A around P
  - if j = i return P
  - if j lt i return Dselect(2nd part of A, n-j, i-j)
  - else return Dselect(1st part of A, j-1, i)
- 2 recursive call: one on n/5 one on unknown
  - laydown the groups of 5 as columns on x axis
  - the westsouth region will be less than the median of the middle group
  - the eastnorth region will be bigger than the median of the middle group
  - => T(7n/10)
- Then prove by induction, by choose a = 10c
### Lower-bound for Sorting
- Every "comparison-based" sorting algorithm has worst-case running time nlogn
- Non-example: Bucketsort, Countingsort, RadixSort
- suppose algorithm always makes k comparisons to correctly sort these n! inputs
- across all n! possible inputs, algorithm exhibits le 2^k distinct executions
- by pigeonhole, n! > 2^k then one of the comparison result has 2 of n! possible inputs in, therefore wrong sorting. 2^k ge n! => k ge O(nlogn)
### Graphs
- represent pair-wise relationships
- 2 ingredients
  - vertices aka nodes (V)
  - edges (E) = pairs of vertices
    - can be undirected (unordered pairs) or directed (ordered pair) aka arcs
#### Cuts of Graphs
- Def: a cut of a graph (V,E) is a partition of V into two non-empty sets A and B
- Crossing edges of a cut (A,B) are those with 
  - one endpoint in each of (A,B)
  - tail in A, head in B
#### The Minimum Cut Problem
- Input: an undirected graph G=(V,E)
- Goal: compute a cut with fewest number of crossing edges
- application: identify network bottlenecks
- community detection in social networks
### Sparse vs Dense Graphs
- Let n = # of vertices, m = # of edges
- In most case, m is best case n and worst case n square
- sparse graph, m is O(n)
- dense graph, m is close to O(n^2)
- Adjacency Matrix
    - Represent G by a nxn 0-1 matrix A where Aij = G has an i-j edge
	- Variants
		- Aij = # of edges
		- Aij = weight of i-j edge
		- Aij denote direction of edge
- Adjacency List
	- array of vertices
	- array of edges
	- each edge points to its endpoints
	- each vertex points to edges incident on it (one hop out)
### Random Contraction 
- Solve minimum cut problem 
- To compute the success probability
- Key Observation: degree of each vertex is at least k (degress=# of incident edges)
	- Reason: 


### Python's f-string
- format string with expressions, methods
```python
>>>name = "ALFRED"
>>>f"My name is {name.lower()}."
'My name is alfred.'
```

### Data Abstraction
- Assignment statement is a type of data abs
- Function also
- Visibility
    - Public and Private
	- Constructor almost always public
- Steps for Data Abstraction
  - Specify
	- Precondition for correctness
	  - REQUIRES
	- What does it change
	  - MODIFIES
	- What does the method DO 
	  - EFFECTS
  - Use
	- think of all the range of usage scenarios
  - Test
    - Black Box Testing is testing w/o knowing any of the internal structure of the system being tested, and we are interested in testing the functionality of the software.
	- White Box Testing is actually testing the internal structure of the system. want to test the actual details of the implementation. 
	- J Unit
	  - annotations @Test @Before 
	  - assertTrue(t.isFacingLeft());
	  - if assert fails, the test will be stopped at that line and will not execute the rest
	- Testability
	  - J Unit is only seeing the public interface of the abstraction, since its a outside caller into the abstraction, sol it can't check the implementation details
	  - Can't check the field private to the abstraction
	  - This is Black Box Testing
	  - Debugging
	    - Typo
		- Should introduce bugs (mutant) to see if test still pass to test for coverage
  - Implement
    - first figure out the internal representation
	  - e.g  collection: list, sets
	  - list: arraylist, linked list
	  - sets: hashset, treeset
### Algorithm
#### Motivation
- Check if a network is connected
- Driving direction
- Can think of paths as sequence of decisions, formulate a plan
- Sudoku puzzle can be solved as nodes can be partially completed puzzle
#### Generic Graph Search
- Goals
  1. find everything findable from a given start vertex
  2. don't explore anything twice
- Generic Algo (given graph G, vertex S)
  - initially s explored, all other vertices unexplored
  - while possible:
    - choose an edge (u,v) with
	- u explored and v unexplored, if none, halt
	- mark v explored
	- v is explored iff G has a path from s to v
#### BFS
- explore nodes in "layers"
- can compute shortest paths
- can compute connected components of an undirected graph
- using a queue (FIFO)
#### DFS
- explore aggressively like a maze, backtracking only when necessary
- compute topological ordering of directed acyclic graph
- compute connected components in directed graphs
- using LIFO stack or via recursion	
