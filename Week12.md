# PorgramLang
### OOP vs FP
- Organize the program "by rows" or "by columns"
- Depends on what you are doing
- E.g GUI OOP more "natural"
- What if we want to extend our program
  - if you expect new operation, use FP
  - if you expect new variant, use OOP
  - but extensibility is double-edged sword
- OOP: Double Dispatch
  - 
- Multimethod
  - 
- Mulitiinheritance
  - allow more than one superclass
  - e.g ColorPt3D from ColorPt and 3D
  - Single inheritance: the class hierarchy is a tree
  - Multiple: Diamonds
- Mixins
  - a collections of method
  - ruby keyword module
  -
- Interfaces
  - In Static typing like Jave/C#
  - subtype = subclass and can pass intance of a subclass to a method expect superclass
  - interface is a type
    - doesnt not contain def, only contain signatures
	  - unlike mixins
	- cannot use new on an interface
	  - like mixins 
- Abstract Method
  - k
# HtCx2
### Accumulators
- the structural recursion make us lost track of where we are
- the context preserveing Accumulators keep track of this info, by acc and invariant 
### Tail
- Tail position
  - The result of the tail position of express is the result of the func
  - if the recursion is not in tail pos, then will build up pending operation
- Tail recursion
  - put the recursive call in the tail position
### Worklist accumulator
- Every single recursive call has to be in tail position
- Usually one of the function in mutually recursive func change the value of the acc
```rkt
(define (count w)
  ;; rsf is Natural; the number of wiz seen so far 
  ;; todo is (listof Wizard ; wizards we still need to visit with fn-for-wiz
  ;; (count Wk)
  ;; (fn-for-wiz Wk 0)
  ;; (fn-for-wiz Wk 0)
  (local [(define (fn-for-wiz w todo rsf)
			 (fn-for-low (append (wiz-kids w) todo) ; Depth-first
						 (add1 rsf)))
		  (define (fn-for-low todo rsf)
			(cond [(empty? todo) rsf]
		try		  [else
				   (fn-for-wiz (first todo) (rest todo) rsf)]))]
	(fn-for-wiz w empty 0)))
```


