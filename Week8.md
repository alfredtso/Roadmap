# Intro to OS 
### Basic C Structure
- Array
  - Static data struct, The length cannot be altered at run time
- Linked list
  - Can be Arbitrary Sized, so its a dynamic data struct
```c
struct testing
{
	int val;
	struct testing *next;
};
```
- The next pointer of the last node would contain a NULL
- A node should be created by malloc memory to the struct
```c
struct testing *node = (struct testing *) malloc(sizeof(struct testing));
```
Simple OS definition:
- a special piece of software that
  - Abstracts: To simplify what the hardware actually looks like
  - Arbitrates: To oversee to control the hardware use
- Metaphor: A shop manager
   - Directs ops resources
     - control use of CPU etc
   - Enforce working policies
   - Mitigate difficulty of complex task
- The operating sys also sit between applications and hardwares
  - hide hardware complexity
    - read/write file, while storage deals with bits within disk
  - resources management
	  - CPU scheduling
  - provide isolation and protection
##### OS Element example
- Abstraction
  - memory page
- Mechanism
  - allocate, map to a process
- Policies
  - least recently used
#### Design Principles
Separation of Mechanism and Policies
- implment flexible mech to support many policies
- e.g LRU, random
Optimize for the common case
- What will the user want to execute on that machine?
#### User/Kernel Protection Boundary
- Kernel-level has privileged access to hardware
- user-kernel switch is supported by hardware
  - trap instructions: privileged bit
  - system call
  - signals
- This is necessary for application to perform certain task need to access hardware
### Monolithic OS
- Pros: everything included
- Cons: large
### Modular OS
- How linux works: better and more common
### Microkernel
- embedd system more common
- Very small in size
- portability is not good: specialized
- complexity
- cost of switching user/kernel mode

# HtCx2
### Mutual Reference
- produce type comment and template next to each other 
- Natural Mutual Recursion
- Backtracking
  - use "if not false to see if the first child is successful"
  - otherwise proceed to rest of childrean
  - thus "backtracking", for signature return "something or False"
