# Stream
- is an infinite sequenece of values
- use a thunk to delay creating most of the seq
- represent streams using pairs and thunks
- Every stream is thunk and when called returns a pair, the cdr part is a thunk using recursion. thunk in thunk

### Example
- if you have a bunch of user-event, like mouse-click, unix-pipe

```
#lang racket

; 1 1 1 1 1 1 ...

(define ones (lambda () (cons 1 ones)))

; 1 2 3 4 5

(define (f x) (cons x (lambda () (f (+ 1 x)))))
(define nats (lambda () (f 1)))
(define nats1
  (letrec ([f (lambda (x) (cons x (lambda () (f (+ x 1)))))])
    (lambda () (f 1))))

; 2 4 8 16 ...

(define powers-of-two
  (letrec ([f (lambda (x) (cons x (lambda () (f (* x 2)))))])
    (lambda () (f 2))))

(define (numbers-til stream tester)
  (letrec ([f (lambda (stream ans)
                (let ([pr (stream)])
                  (if (tester (car pr))
                      ans
                      (f (cdr pr) (+ 1 ans)))))])
    (f stream 1)))

(define (stream-maker fn arg)
  (letrec ([f (lambda (x) (cons x (lambda () (f (fn x arg)))))])
    (lambda () (f arg))))
```

