# ProgramLang
### Static Checking
- def: anything done to reject the prog after it parses but before it runs
- Compile Time and Run Time checking are just 2 pts on the continum
### Correctness
- type system is sound if it prevent X for some X 
  - no false negatives
- type system is complete if it never reject program that wont do X
  - no false positives
- usually sound but not complete enough
- any static checker cannot do all of:
  1. always terminate
  2. be sound
  3. be complete
### Weak Typing
- Some program must pass static checking but can set computer on fire
- Ease of lang implementation
- Performance
- C and CPP, one big problem is array bounds
### Dynamic typing vs Static typing
- Convenience
- not preventing useful programs
- catching bus early
- performance
- code reuse
### Eval
- at run-time create some data
- then treat the data as a program and run it
### quote
- reverse of eval
- everything underneath atoms and lists, not variable and calls

# HtCx2
### Abstraction from example
- identify 2 highly repetitive exp introduce a new function
- around one copy of repetitive code
- with a more general name
- add a para for varying position
- use para in varying position
- replace specific expression
- with calls to abstract function
- pass in varying value

```rkt
(foldr + 0 (list 1 2 3))
```
- foldr
- 0 is the base case and + is combination
- (build-list 4 sqrt)
- when you use built-in abstract fn, we dont need to test base case 
### Closure
- because local function will be renamed and lifted, each time a value w passed to local, a special version of the local fn will be created at top level
### Foldr
- its convention to put function arguements first when designing abstract fn
- fun example: copy-list
```rkt
(define (copy-list lom) (fold cons empty lom))

(define (fold-element c1 c2 b e)
  (local [(define (fn-for-element e)
			(c1 (elt-name e)
				(elt-data e)
				(fn-for-loe (elt-subs e))))

		  (define (fn-for-loe loe)
			(cond [(empty? loe) b]
				  [else
				   (c2 (fn-for-e (first loe))
					   (fn-for-loe (rest loe)))]))]
    (fn-for-element e)))
```
- first arg c1 is a function of 3 args (String, Integer, Y -> X)
- second c2 is a function of 2 args (X, Y -> Y)
- b is result of loe so its Y
- result is fn-for-element which is X
### CLI
- Harddisk
- diskutil
### Linux
- Performance
  - Preload
  - I/O scheduler


