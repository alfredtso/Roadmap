# HtCx2
## Two One-of Type
### Cross Product Table Type Comment
- write down the case in table, and obtain at least how many cases 
- simplify the case and try to look for natural recursion in the model level
### Local
- Lexical Scope
  - look at the "inner most" environment for the binding
- Scope contour
- Evaluation rules
  - renaming
  - lifting
  - replace local with body
```rkt
(define b 1)

(+ b
   (local [(define b 2)]
      (* b b))
	b)

(+ 1
   (local [(define b 2)]
      (* b b))
	b)

(define b_0 2)				; lifting

(+ 1						
   (local [(define b_0 2)]	; rename
      (* b_0 b_0))
	b)

(define b_0 2)			; replace local with body

(+ 1						
    (* b_0 b_0))
	b)

(define (foo x)
  (local [(define (bar y) (+ x y))]
    (+ x (bar (* 2 x)))))

(list (foo 2)(foo 3))

...

(list 8 (foo 3))

(list 8 (local [(define bar y) (+ 3 y))]
		  (+ 3 (bar (* 2 3))))

(define (bar_1 y) (+ 3 y))
(list 8 (+ 3 (bar_0 (* 2 3))))

(define (bar_1 y) (+ 3 y))
(list 8 (+ 3 (bar_0 6)))

(define (bar_1 y) (+ 3 y))
(list 8 (+ 3 (+ 3 6)))

(define (bar_1 y) (+ 3 y))
(list 8 (+ 3 9))

(list 8 12)
```
### Encapsulation
- helper functions are encapsulated inside the function
- trampoline 
- refactoring
- sometimes first refactoring and then change the behaviour
- local helper can have "funny" names since outside wont be able to access it
#### Avoid recomputation
- use local binding to avoid recomputation result, just like in ProgramLang

# ProgramLang
### Implementing Variables
- An enviroment is a mapping from varialbes (Rkt string) to values, pairs of strings and values in the environment
### Implementing Closure
- when we create the function we have to create a closure that include the env 
```
(struct closure (env fun) #:transparent)
```
- Evaluation a function exp:
  - a function is not value: a closure is a value
  - evaluatgging a function return a closure
  - create a cloure out of the function and the current env when the function was eval
- Function calls 

# Nand2Tertis
### Computer Architect
- - Memory Hierachy
- CPU usually contain a few "registers"
  - Data Registers
  - Address Registers
    - the address of the main memory which the result of the data register will write to given by the address registers
    - addressing modes
	  - register
	  - direct
	  - indirect
	  - immediate
- Flow control
  - in sequence
  - conditional jump
  - unconditional jump
### Hack Machine Language
- Basic 
![Hack Computer]()
- 16-bit A-instructions
- 16-bit C-instructions
- Control:
  - The Rom is loaded with a hack program
  - the reset is pushed
  - program starts running
- Hack machine language recognize three registers
  - D and A reside in CPU and M is in Ram
  - all hold 16-bit value
  - M is the ram reg addressed by A
- A-instruction
	- @value 
	- set the A register to  value
	- RAM[value] becomes the selected RAM register
	- always need to set the address first and then operate on
- C-instruction
```
dest = comp ; jump 
```
    - both dest and jump are optional
	- comp has a set of ops to select from
#### I/O
- Keyboard
- Screen
- Screen Memory Map
- continuously refreshed from the memory map from the Ram
- to set pixel on/off
1. word = Screen[32*row + col//16], world = RAM[16384 + 32*row + /col/16]
2. set the (col%16)th bit of word to 0 or 1
3. Commit word to the Ram
- keyboard memory map
1. a key is pressed, a key's scan code appear in the keyboard memory map
2. to see the contents of the keyboard chip
3. in the hack computer: probe the contents of RAM[24576]
4. if 0, no key is pressed
### Hack Programming
- Will need assembler to translate assembly program to binary code
- CPU emulator can be used to debugging and executing 
#### Working with registers and memory
- D, A, M: the currently selected memory, M=RAM[A]
```asm
// D=10
@10
D=A

//D++
D=D+1

// D=RAM[17]
@17
D=M

// RAM[17]=10
@10
D=A
@17
M=D

// RAM[5]=RAM[3]
@5
D=M

@3
M=D

// end instructions with infiintie loop
```
- Virtual register
- R5 to denote RAM[5]
- SCREEN and KBD has base address

#### Branching
- Goto
- symbolic reference
- declare a label

#### Variables
- first write pseudo code
- write it in asm language
- test it with a trace table 

#### Pointers
- let say size 10 array starts at 100
```asm
// arr = 100
@100
D=A
@array
M=D

// n=10
@10
D=A
@n
M=D

// initialize i = 0
@i
M=0
```
