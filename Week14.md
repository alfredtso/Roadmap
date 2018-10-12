# SoftConstruction
- Needs to be able to familiar yourself with a large code base asap
- learn to walking up the structure of the program
  - look at files and folders, since unlike HtCx, code scatter in different files
  - run the program to get a feel first
  - relate the app to the code
  - inter-package relationships
    - can also see the inter-class dependency 
  - inter-file relationships
# IntrotoOS
![Process Address Space]()
![Process Address Space]()
![Process Address Space]()
![What is?](https://i.imgur.com/cJwl4nN.png)
![How](https://i.imgur.com/7bV5J02.png)
![Context Swiching](https://i.imgur.com/GKI3jdu.png)
![Life Cycle](https://i.imgur.com/NaMUvwS.png)
![Creation](https://i.imgur.com/0iYJkNL.png)
![CPU scheduler](https://i.imgur.com/Dg2a9G5.png)
![Length of Process](https://i.imgur.com/ZhXB6cf.png)
![What about I/O](https://i.imgur.com/l1mvTHo.png)
![Message-passing](https://i.imgur.com/BKVLt2c.png)
![Shared Memory](https://i.imgur.com/A5doP18.png)
![Threads](https://i.imgur.com/G5Ez8hs.png)

# Algorithm
### Quick Sort
- Key idea: partition array array around a pivot element
- pick element of array
- rearrange array so that:
  - left of pivot less than pivot
  - right of pivor greater than pivot
  - puts pivot in its "right pos" and we doesnt care if the left or right bucket is in order
- Partition
  - its linear time
  - reduces problem in size
- QuickSort:
  - if n = 1, return 
  - p = ChoosePivot(A.n)
  - Partition A round p and then recurse on both side
- Using O(n) extra memory, easy to partition around pivot in O(n) time.
- Assume: pivot = 1 st element of array
- single scan through array
- invariant: everything looked at so far is partitioned
![Pseudocode](https://i.imgur.com/aHpP2uK.png)
#### Correctness
- Claims: the for loop maintains the invariants
![Correctness](https://i.imgur.com/k6CmYiw.png)
#### Randomized Algorithm
- Choose Random Pivot 
- Intuition
  - 25-75 split is good enough for it to run O(n log n) time
  - half of elements (for being pivot) give a 25-75 split or better
![Prelim](https://i.imgur.com/1QlEuoA.png)
##### Building Blocks
- Master method doesnt apply, because size of subproblem is different 
- Use decomposition principle
- decompose the complicated comparison between input elements in all possible sequence to sum of simple  comparison of ith and jth elements which could only take on value zero or 1
#### General Decomposition Principle
- Identity random variable Y that matters
- Express it as sum of indicator random variables:
- Apply linearity of expectation
![Main](https://i.imgur.com/HJ3IuSn.png)
![Decompose](https://i.imgur.com/1os8nkR.png)
![Proof1](https://i.imgur.com/SctcDl0.png)
![Proof2](https://i.imgur.com/Bten7tG.png)

# Networking
![queue](https://i.imgur.com/MaESpkg.png)
