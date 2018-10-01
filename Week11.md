# HtCx2
### Generative Recursion
- structural recursion
  - first los and then recurse rest los
  - each time we take a small piece to op on
- generative recursion
  - each time we generate a new piece of data to op on
  - also do the base case test first
  - in this kind of recursion, next smallest base case help us understand
```rkt
(define (genrec-fn d)
  (cond [(trivial? d) (trivial-answer d)]
        [else
         (... d 
              (genrec-fn (next-problem d)))]))

;; Natural -> Image
;; produce recursive Sierpinski carpet of size s
(define CUTOFF 2)

(check-expect (stri CUTOFF) (Sqaure CUTOFF "outline" "red"))
```
### Termination Arguement
- Three part termination
  - Base case: s smaller thatn CUTOFF
  - Reduction Step: divide s by 2
  - Arguement that repeated application of Reduction step will reach base
### Lambda Expression
- locally defined fn is only used in one place and quite simple
- change the fn to (lambda (arg) (body))
### Sudoku Brute-Force
- fill all possible answer
- prune impossible outcomes
- repeat
- along each branch we either
  - come to a board with no child -> produce false
  - come to a full board and thats the solution
- generate arbitrary-arity tree and do backtracking search over it
### Template-blending
- Heres 3 Template blend together
```rkt
(define (solve bd)
  (local [(define (solve--bd bd)
		    (if (solved? bd)
				bd
				(solve--lobd (next-boards bd))))
		  (define (solve--lobd lobd)
		    (cond [(empty? lobd) false]
			      [else
				   (local [(define try (solve--bd (first lobd)))]
				   (if (not (false? try))
					   try
					   (solve--lobd (rest lobd))))]))]
	(solve--bd bd)))
```

### Hack Programming
- Pointer @ Address
- I/O
- Ram: Data Memory, Screen memory map, keyboard
- SCREEN: base address, KBD: base address
-

# ProgramLang
### OOP
- All values are references to objects
- Given an object, code "communicated with it" by calling its methods
- Each object has a private state and can be accessed only by object's methods
- Every object is an instance of a class
- Instance variables: some lang call it fields 
- Class method: also static methods, can be called from outside C and is defined with C.method args
- 

# SQL
- select: like project in ra
- from
- where
- set operators
  - Union
  - Intersect
  - Except
### Subqueries in Where
- needed because duplicate is a problem
- keywords: (not) in, exist, all, any
