# ProgramLangB
## Racket
- Dynamic typing
  - Can use values of any type anywhere, that's why they have (number?) func
  - since doesnt type check, will only catch error at runtime when being evaluated
### Cond
- Better than nested if-expressions
  - Should always evaluate true at the last expression
  - Racket treat anything other than #f as true
    - Some ppl consider this poor style
    - some languages other things are false
### Local Bindings
- Also has let expression like in ML
- 4 ways to define local variables
  1. let
	- the expression are eval in the env from before the let-expression
	- (let ([x y] [y x]) ...) would proper change x and y which ML would not
  2. let*
	- the exp are eval in the env produced from the prev bindings
	- the ML let 
```rkt
(define (silly-double x)
  (let* ([x (+ x 3)]
		 [y (+ x 2)])
     (+ x y -8)))
```
  3. letrec
    - the exp are eval in the env include all the bindings
  4. define
```rkt

; notice the parentheses in the let-expressions
(define (max-of-list xs)
  (cond [(null? xs) (error "max-of-list given empty list")]
        [(null? (cdr xs)) (car xs)]
        [#t (let ([tlans (max-of-list (cdr xs))])
              (if (> tlans (car xs))
                  tlans
                  (car xs)))]))

; 4 forms of local bindings

; let evaluates all expressions using outer environment, 
; *not* earlier bindings
(define (double1 x)
  (let ([x (+ x 3)]
        [y (+ x 2)])
    (+ x y -5)))

; let* is like ML's let: environment includes previous bindings
(define (double2 x)
  (let* ([x (+ x 3)]
         [y (+ x 2)])
    (+ x y -8)))
  
; letrec uses an environment where all bindings in scope
; * like ML's use of and for mutual recursion
; * you get an error if you use a variable before it's defined
;   where as always function bodies not used until called
;   (bindings still evaluated in order)
; (In versions of Racket before 6.1, you got an #<undefined> value instead,
;  which would typically cause an error as soon as you used it.)
(define (triple x)
  (letrec ([y (+ x 2)]
           [f (lambda (z) (+ z y w x))]
           [w (+ x 7)])
    (f -9)))

(define (mod2 x)
  (letrec 
      ([even?(lambda (x) (if (zero? x) #t (odd? (- x 1))))]
       [odd? (lambda (x) (if (zero? x) #f (even? (- x 1))))])
    (if (even? x) 0 1)))

(define (bad-letrec-example x)
  (letrec ([y z] ; okay to be a lambda that uses z, but here y undefined
           [z 13])
    (if x y z)))

; and you can use define locally (in some positions)
; the same as letrec when binding local variables
(define (mod2_b x)
  (define even? (lambda(x)(if (zero? x) #t (odd? (- x 1)))))
  (define odd?  (lambda(x)(if (zero? x) #f (even? (- x 1)))))
  (if (even? x) 0 1))
```
### Set-bang
- top-level binding is usually no good
  - because if you can mutate, you need to make local copy of all the stuff
### Cons
- list is cons with null at the end 
- if no, thats pair
- because dynamic type, dont have type checker, so might as well use the same
- Style
  - proper lists for unknow size
  - otherwise pairs will be good 
### mcons
- Mutable cons
### Delayed Evalutaion and Thunk
- Function arguement are evaluated before the call
- Conditional branches are not eager (not all being evaluated for example)
- Thunks delay
  - to delay evalutaion, put the expression in a function (lambda)
- e
  - eval an exp to get a result
- (lambda () e)
  - eval when called and return result, zero-argument function for thunking
- (e)
  - Eval e to some thunk and then call the thunk
### Avoiding Unnecessary computation
- Thunk let you skip expensive computatuins if they are not needed
- remember some expensive computation so can reuse is call lazy eval
### Delay and force
```rkt
(define (my-delay th)  ; since no paren, th is not being called, -> fast
  (mcons #f th))

(define (my-force p)
   (if (mcar p)
	   (mcdr p)
	   (begin (set-mcar! p #t)
			  (set-mcdr! p ((mcdr p)))
			               -        -   ;these paren actually call th
										;and set-mcdr store result in mcdr
			  (mcdr p))))

(my-mult x (let ([p (my-delay (lambda () (slow-add 3 4)))])
			 (lambda () (my-force p))))

; this is a silly addition function that purposely runs slows for 
; demonstration purposes
(define (slow-add x y)
  (letrec ([slow-id (lambda (y z)
                      (if (= 0 z)
                          y
                          (slow-id y (- z 1))))])
    (+ (slow-id x 50000000) y)))

; multiplies x and result of y-thunk, calling y-thunk x times
(define (my-mult x y-thunk) ;; assumes x is >= 0
  (cond [(= x 0) 0]
        [(= x 1) (y-thunk)]
        [#t (+ (y-thunk) (my-mult (- x 1) y-thunk))]))

; these calls: great for 0, okay for 1, bad for > 1        
;(my-mult 0 (lambda () (slow-add 3 4)))
;(my-mult 1 (lambda () (slow-add 3 4)))
;(my-mult 2 (lambda () (slow-add 3 4)))

; these calls: okay for all
;(my-mult 0 (let ([x (slow-add 3 4)]) (lambda () x)))
;(my-mult 1 (let ([x (slow-add 3 4)]) (lambda () x)))
;(my-mult 2 (let ([x (slow-add 3 4)]) (lambda () x)))

(define (my-delay th) 
  (mcons #f th)) ;; a one-of "type" we will update /in place/

(define (my-force p)
  (if (mcar p)
      (mcdr p)
      (begin (set-mcar! p #t)
             (set-mcdr! p ((mcdr p)))
             (mcdr p))))

; these calls: great for 0, okay for 1, okay for > 1
;(my-mult 0 (let ([x (my-delay (lambda () (slow-add 3 4)))]) (lambda () (my-force x))))
;(my-mult 1 (let ([x (my-delay (lambda () (slow-add 3 4)))]) (lambda () (my-force x))))
;(my-mult 2 (let ([x (my-delay (lambda () (slow-add 3 4)))]) (lambda () (my-force x))))
```
### Streams
- an infinite seq of values
- cannot make a stream by making all val 
  - key idea: use thunk to delay creation of most of the sequence
- a powerful concept for divison of labor:
  - Stream producer knows how to create any number of val
  - comsumers knows how many val to ask for
  - e.g UNIX pipes, mouse clicks
- represent streams using pairs and thunks
```rkt
(define powers-of-two
  (letrec ([f (lambda (x) (cons x (lambda () (f (* x 2)))))])
    (lambda () (f 2))))

(define (stream-maker fn arg)
  (letrec ([f (lambda (x)
                (cons x (lambda () (f (fn x arg)))))])
    (lambda () (f arg))))
(define nats2  (stream-maker + 1))
(define powers2 (stream-maker * 2))

;; code that uses streams

(define (number-until stream tester)
  (letrec ([f (lambda (stream ans)
                (let ([pr (stream)])
                  (if (tester (car pr))
                      ans
                      (f (cdr pr) (+ ans 1)))))])
    (f stream 1)))

(define four (number-until powers-of-two (lambda (x) (= x 16))))
```
#### Making Streams
```rkt
(define ones-bad (lambda () (cons 1 (ones-bad))))

(define ones (lambda () (cons 1 ones)))

(define nats
  (letrec ([f (lambda (x) (cons x (lambda () (f (+ x 1)))))])
    (lambda () (f 1))))
```
### Memoization
- An idiom related to lazy evaluation doesnt use thunk
- if func has no side effects, we can keep a cashe of previous results, since calling twice is same
- similar to delay and force, but they have no arguement since they pair
```rkt
(define fibonacci3
  (letrec([memo null] ; list of pairs (arg . result) 
          [f (lambda (x)
               (let ([ans (assoc x memo)])
                 (if ans 
                     (cdr ans)
                     (let ([new-ans (if (or (= x 1) (= x 2))
                                        1
                                        (+ (f (- x 1))
                                           (f (- x 2))))])
                       (begin 
                         (set! memo (cons (cons x new-ans) memo))
                         new-ans)))))])
    f))
```

# Marcos

- decribe how to trans some new syntax into different syntax in the source lang
- a way to implement syntastic sugar: x1 andalso x2 same if x1 then x2 else false
- A macro system is a language for defining macros
- Macro expansion is the process of rewriting the syntax for each marco use before a prog is run
```rkt
;; a cosmetic macro -- adds then, else
(define-syntax my-if
  (syntax-rules (then else)
    [(my-if e1 then e2 else e3)
     (if e1 e2 e3)]))

;; a macro to replace an expression with another one
(define-syntax comment-out
  (syntax-rules ()
    [(comment-out ignore instead) instead]))

; makes it so users don't write the thunk when using my-delay
(define-syntax my-delay
  (syntax-rules ()
    [(my-delay e)
     (mcons #f (lambda () e))]))

; some uses

(define seven (my-if #t then 7 else 14))
; SYNTAX ERROR: (define does-not-work (my-if #t then 7 then 9))
; SYNTAX ERROR: (define does-not-work (my-if #t then 7 else else))

(define no-problem (comment-out (car null) #f))

(define x (begin (print "hello") (* 3 4)))

(define p (my-delay (begin (print "hi") (* 3 4))))

(define (my-force th)
  (if (mcar th)
      (mcdr th)
      (begin (set-mcar! th #t)
             (set-mcdr! th ((mcdr th)))
             (mcdr th))))
```
### Tokenization, Parentiesization, and Scope
Token
- meaning a word rather than character
- e.g macro expand head to car
Parenthesization
- How does associativty work
- because rkt already ho extra on these so okay
Local bindings
- Can variable shadow macros
- Racket local variable will shadow the macro
	
### define syntax
```rkt
(define-syntax name-of-marco
  (syntax-rules (other-key-word-of-macro)
   [(name-of-macros e1 then e2 else e3)
    (if e1 e2 e3)]))
```

