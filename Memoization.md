# Memoization
- If a fx has no side-effect and does not read mutable memory, dont recomp.
- can keep a cache of previous results
- Win if maintaingin cache is cheaper than recomp and results are resused
- Similiar to promises, but if fx takes arg, there are multiple "prev res"

```rkt
(define fibonacci3
  (letrec ([memo null] ; list of pairs (arg . result)
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
