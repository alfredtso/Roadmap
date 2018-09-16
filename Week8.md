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

# Nand2Tertis
### Sequential Logic
- So far what we built is "combinatorial", not involve time state
- no time state, we cannot remeber stuff thus no memory
- Standard means
  1. The Clock
  - The passage of time is represented by a master clock that delivers a continous trains of alternating signals, the hardware implementation is based on an oscillator alternates cont. between low-high, tick-tock and the elapsed time between the beginning of the tick and the end of the tock is called cycle and each clock cycle is taken to be one discrete time unit
  2. Flip-Flops
  - The most Elementary sequential element
  - DFF has a clock that changes according to the master clock and interface consists of one bit output and one bit input
  - DFF implement out at time t equals in at time t-1 
  3. Register
  - We could feed the output back to the input of DFF through a Mux with the select bit being the "load" bit and then we will have a bit register and stacking them up can form words
- RAM
  - Three parts
  - A data input
  - an address input
  - a load bit
  - the basic designe parameter of a RAM device are its data width, the width of each words (typical 32-bits 64 bits for modern computers) and size (number of words in the RAM) hundreds of millions of it.
- Counters
  - increment the input by 1
  - usually should be able to reset to zero
  - loading a new loading base
  - decrementing
- Time Matters
  - In combinational chips, we cannot use feedback loop since "data races" but in sequential there is a inherent timedelay so the output at time t does not depend on itself rather on the output of t-1
  - We allow sequential chips to be in unstable state during clock cycles, only ta the beginging of the next cycle they need to ouput the correct value
  -
