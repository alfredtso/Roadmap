- Declarative Programming
- Versus imperative, just say what you want and let compyter figure out how to get 
- We want to move towards more declarative
- is also the use of math in programming 
- also called programming without state, versus imperative: stateful
- "program works today will work tmr"
- Concept of identifier: is acharacter string used to reference entities during the execution of a program
- Concept of variable in mem: in Oz, var are single assignment, FP essential
- Concept of environment: is the correspondence betw the identifiers in the program and the var in mem. At every point in a program's execution, there is a well-defined environment. An env is also a function of one arg takes an identifier and return a var in mem.
- Static scoping
- you can redeclare identifier but only the last declaration matters and this is different from multuple assignment
- tail recursion using accumlator
- principle of communicating vases: decrese a variable and increase another same time
- 2 arguement, one is accumulator and the recursive call is the last ops in the fun body
- recursive fun is equ to a loop if tail recursive
- we find the accumulator starting from an invariant using the principle of communicating vases
- this is invariant programming and it is the only reasonable way to program loops
- think loops in terms of invariant, and changing one part of the invariant forces the rest to changeas well
- <List T> ::= nil | T '|' <List T>
- <obtree T> ::= leaf | tree(key:T value:T left:<obtree T> right:<obtree T>)
- Search Tree
	- Lookup 
	- Insert 
	- Delete 
- Spatial complexity
	- execution time is temporal complexity
	- memory yse is spatial complexity
		- activve memory: total number of words in use by program at t
		- memory comsumption: number of words allocated per second at time t 
### Semantics
1. translate the program into kernel language
2. exe the translated program on the abstract machine
	- {Fact 0 R} on the abstract machine 
	- the contextual environment of a fun contains all the identifiers that are used  inside the fun but declared outside of the fun
	- the procedure value is a closure which is a pair whose values are procedure code and the contextual environment(free variables) stored in memory

### Abstract Machine
Has 6 concepts
- Memory
- Environment
- Semantic Instruction
- Semantic stack
- Execution state
- Execution

#### Example
```
local X in
  local B in
	B=true
	if B then X=1 else skip end
  end
end
```
```

([local X in
   local B in
	B=true
	if B then X=1 else skip end
  end
end,{}] // {} is empty env
{}) // {} is empty mem.

([local X in
   local B in
	B=true
	if B then X=1 else skip end
  end
end,{X->x}] 
{x})  

// create a new variable x in memory
// put the inner instruction on the stack and add X->x to its env

([ 	B=true
	if B then X=1 else skip end
  end
end,{B->b, X->x}] 
{b, x}) 
// same as what happen to local X in the above

([ 	(B=true, {B->b, X->x}),
	(if B then X=1 else skip end, {B->b, X->x})],
{b, x}) 
// split the sequential compositoin into its two parts and put the 2 instruction on the stack 


([	(if B then X=1 else skip end, {B->b, X->x})],
{b=true, x}) 
// bind b to true in mem

([	(X=1, {B->b, X->x})],
{b=true, x}) 
// read the value of B, and since B is true, we put the instruction after then on the stack
// if B is false, then put after else
// other value of B conditional raises an error

([],
{b=true, x}) 
// bind x to 1 and execution stops because the stack is empty
```

- we have seen 4 instruction
```
local <x> in <s> end (variable creation)
<s1> <s2> (sequential compositoin)
if <x> then <s1> else <s2> end (conditional
<x> = <v> (assignment)
``` 

#### Semantic rules for kernel language instruct
- Each instruction we will define rule in the abstract machine
- Each instruction takes one execution state as input and returns one execution state
	- Execution state = semantic stack + memory
- Skip
	- input state: ([skip, E), S2, ...., Sn], sigma)
	- Output state: ([S2, ...,Sn], sigma)
- sequential composition
	- Input state:([(Sa,Sb), S2, ...,Sn], sigma)
	- Output state:([Sa, Sb, S2, ...,Sn], sigma)
- local x in s end
	- create a fresh new variable x in memory sigma
	- Add the link {X->x} to the environment E 

#### Dyanmic scoping versus static scoping
```
local P Q in
	proc {Q X} {Browse stat(X)} end
	proc {P X} {Q X} end
	local Q in
		proc {Q X} {Browse dyn(X)} end
		{P hello}
	end
end
```

#### Single-assignment store
- a set of store variable
	- 1. sets of var that are equal but unbound
	- 2. var bound to a number, record or procedure
	- e.g {x1, x2=x3, x4 = a|x2} meaning x1 is unbound and x2 x3 are equal and unbound and x4 is bound to partial value

- An **Environment E** is a mapping from variable id to entities in memory, as a set of pairs e.g{X->x, Y->y}, where X,Y are id and x,y refer to store entities
- A **semantics statement** is a pair (S, E), S is a statement and E is env, the semantics statement relate S to what it references in the store.
- An **execution state*** is a pair (ST, sigma), ST is a stack of semantic statements and sigma is single-assign store
- A **computation** is a seq of execution states
- a single transition in a computation is called a computation step. A step is atomic meaning there are no visible immediate states, as if its done "all at once"
- Semantic stack can be in one of three run-time states:
	- Runnable: ST can do a computation step
	- Terminated: is empty
	- Suspended: not empty, but cannot do any commputation step

### Nonsuspendable statements
1. **skip** statement
	- 

