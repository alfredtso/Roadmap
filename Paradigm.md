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
