# Week4
- [ProgramLangA](#programlanga)
- [HtC1x](#HtC1x)

# ProgramLangA
## Functional programming
- Aviod mutation
- Using function as values
- Encourage
  - Use of recursion and recursive data struct
  - closer to math def
  - idioms using laziness
### First-class functions
- Can use them wherever we use values
```sml
fun double z = 2 * x
fun incr x = x + 1
val a_tuple = (double, incr, double(incr 7))
val eighteen = (#1 a_tuple) 9
```
- Most common use is use as an argument/ reult of another function
  - Other function is called a higher-rorder function
  - Powerful idiom to factor out common functionality
- Function closure : functions can use bindings from outside the func def
#### Functions as arugments
- Elegant for factoring out common code
  - Replace N similar functions with 1 function have the common part
```sml
fun n_times(f,n,x) = 
	if n = 0
	then x
	else f(n_times(f,n-1,x))

fun increment x = x + 1
fun double x = x + x

val x1 = n_times(double,4,7)
val x2 = n_times(double,4,[1,2,3])

fun addition (n,x) = n_times(increment,n,x)
fun double_n_times(n,x) = n+times(double,n,x)

fun triple_n_times(n,x) = n_times(triple,n,x)
fun triple x = 3*x
```
#### Polymorphic function as arguement
- Higher order function are often polymorphic
  - But some not
- n_times take 'a -> 'a rather than int or int list
- We instantiates 'a with both int and in list in above code example
One example of higher order is not polymorphic
```
fun times_until_zero (f,x) =
	if x=0 then 0 else 1 + times_until_zero(f, fx) // we compare x to 0

fun len xs =
	case xs of
	   [] => 0
	  |_::xs' => 1 + len xs'
```

#### Anonymous Funtions
```sml
fun triple_n_times (n,x) =
	let fun triple x = 3 * x 
	in
		n_times(triple, n,x)
	end

fun triple_n_times (n,x) =
	n_times(let fun triple x = 3 * x in triple end, n, x)

fun triple_n_times (n,x) =
	n_times(fn x => 3 * x in triple end, n, x) // fn x is an expression
```
Most common use
- argument to a higher order function
But cannot use it for recursive function
- if not for recursion, fun bindings would be syntactic sugar for val bindings

#### Unnessary Function Wrapping
```sml
fun nth_tail (n,xs) = n_times((fn y => tl y),n,xs)

fun rev xs = List.rev xs (* poor style *)
fun rev = List.rev 
```
fn x => x is Unnessary

#### Map and Filter
```sml
fun map(f,xs) =
	case xs of
	   [] => []
	  |x::xs' => (f x)::map(f, xs')

val x1 = map((fn x => x+1), [3,4,5,6])
val x2 = map(hd, [[1,2]],[3,4]])

fun filter(f,xs) =
	case xs of
		[] => []
	| x::xs' => if f x
				then x::(filter (f xs')
				else filter (f xs')

fun is_even v =
	(v mode 2 = 0)

fun all_even xs = filter(is_even, xs)

fun all_even_snd xs = filter((fn (_,v) => is_even v), xs)
```

val map = fn : ('a -> 'b) * 'a list -> 'b list
val filter = ('a -> bool) * 'a list -> 'a list

#### Generalizing
- take one func as an argument to another function
- take func return func
```sml
fun doubleortriple f =
	if f 7
	then fn x => 2*x
	else fn x => 3*x

fun double = doubleortriple (fn x => x - 3 = 4)
fun nine = (doubleortriple (fn x => x = 42) 3)
```
val doubleortriple = fn : (int -> bool) -> int -> int
- t1 -> t2 -> t3 -> t4 means t1 -> (t2 -> (t3 -> t4))

Higher order func are not just for numbers and list 
- work great for common recursive traversals over your data struct or bindings too
```sml
datatype exp = Constant of int 
	     | Negate of exp 
	     | Add of exp * exp
	     | Multiply of exp * exp

fun true_of_all_constants(f,e) =
    case e of
	Constant i => f i
      | Negate e1 => true_of_all_constants(f,e1)
      | Add(e1,e2) => true_of_all_constants(f,e1)
		      andalso true_of_all_constants(f,e2)
      | Multiply(e1,e2) => true_of_all_constants(f,e1)
			   andalso true_of_all_constants(f,e2)

fun all_even e = true_of_all_constants((fn x => x mod 2 = 0),e)
```
### Lexical Scope
**Funtion can be passed around in where the function was defined**
- This semantics is called lexical scope
```sml
(* 1 *) val x = 1
	(* x maps to 1 *)
(* 2 *) fun f y = x + y
	(* y maps to a function that adds 1 to its argument *)
(* 3 *) val x = 2
	(* x maps to 2, which shadows x but has no effect on f defined above *)
(* 4 *) val y = 3
	(* y maps to 3 *)
(* 5 *) val z = f (x + y) (* call the function defined on line 2 to 5)
	(* z maps to 6, look up f which add 1 to argument and x = 2 and y = 3
		so its f(5) and f add 1 to 5 so 6*)
```
#### Closure
The language implementation keeps old environment around as necessary
- Functions has two parts
  - The code
  - The environment that was **current whenthe function was defined**
    - This pair is called a **function closure**
	- a call eval the code part in the environment part
- Line 2 creates a closure and binds f to it
  - Code: "take y and have body x+y"
  - Env: "x maps to 1"
    - Plus whatever else is in the scope, including f for recursion
- Line 5 calls the closure defined in line2 with 5
  - So body eval in environment "x maps to 1" extended with "y maps to 5"

### Lexical scope and higher order function
```sml
val x = 1
fun f y = 
    let 
        val x = y+1
    in
        fn z => x + y  + z (* take z and return 2y + 1 + z *)
    end
val x = 3 (* irrelevant *)
val g = f 4 (* return a function that adds 9 to its argument *)
val y = 5	(* irrelevant *)
val z = g 6 (* get 15*)
(* if we cal another f 8, it would create another environment *)

(* second example *)
fun f g = 
    let 
        val x = 3 (* irrelevant *)
    in
        g 2
    end
val x = 4
fun h y = x + y (* add 4 to its argument *)
val z = f h (* 7 *)
```
### Motivation: Lexical Scope
Lexical scope: use env where func is defined
Dynamic scope: use env where func is called 
1. Function meaning does not depend on varibale names used
```sml
fun fy =
	let val x = y+1
	in fn z => x+y+z end

can change how x to whatever and doesnt matter
```
2. Functions can be type-checked and reasoned about where its defined
3. Closure easily store the data they need
```sml
(* Being able to pass closures that have free variables (private data)
   makes higher-order functions /much/ more useful *)
fun filter (f,xs) =
    case xs of
	[] => []
      | x::xs' => if f x then x::(filter(f,xs')) else filter(f,xs')

fun greaterThanX x = fn y => y > x

fun noNegatives xs = filter(greaterThanX ~1, xs)

fun allGreater (xs,n) = filter (fn x => x > n, xs)
```
exception handling is more like dynamic scope
- look at the current innermost handler

### Closure and Recomputation
- A function body is not eval until is called
- A function body is eval everytime is called 
- A variable binding eval its expression when the bind is eval, not every time is used
```sml
fun filter (f,xs) =
    case xs of
	[] => []
      | x::xs' => if f x then x::(filter(f,xs')) else filter(f,xs')

fun allShorterThan1 (xs,s) = 
    filter (fn x => String.size x < (print "!"; String.size s), xs)

fun allShorterThan2 (xs,s) =
    let 
	val i = (print "!"; String.size s)
    in
	filter(fn x => String.size x < i, xs)
    end

val _ = print "\nwithAllShorterThan1: "

val x1 = allShorterThan1(["1","333","22","4444"],"xxx")

val _ = print "\nwithAllShorterThan2: "

val x2 = allShorterThan2(["1","333","22","4444"],"xxx")

val _ = print "\n"
```

### Fold and More closures
- synonyms: reduce, inject
  - repeatedly applying f to answer so far
```sml
fun fold (f,acc,xs) = 
	case xs of
		[] => acc
	  | x::xs' => fold(f, f(acc,x), xs)
	
val fold = fn: ('a * 'b -> 'a) * 'a * 'b list -> 'a
```
- it is "iterator like"
```sml
fun fold (f,acc,xs) =
    case xs of 
	[] => acc
      | x::xs' => fold (f,f(acc,x),xs')

(* examples not using private data *)

fun f1 xs = fold ((fn (x,y) => x+y), 0, xs) (* sum list *)

fun f2 xs = fold ((fn (x,y) => x andalso y >= 0), true, xs) (* all list e non-neg *)

(* examples using private data *)

(* counting the no. of elements between lo and hi, inclusive *)
fun f3 (xs,lo,hi) = 
    fold ((fn (x,y) => 
	      x + (if y >= lo andalso y <= hi then 1 else 0)),
          0, xs)

fun f4 (xs,s) =
    let 
	val i = String.size s
    in
	fold((fn (x,y) => x andalso String.size y < i), true, xs)
    end

(* do all the e in list produce true when passed to g *)
fun f5 (g,xs) = fold((fn(x,y) => x andalso g y), true, xs)

fun f4again (xs,s) =
    let
	val i = String.size s
    in
	f5(fn y => String.size y < i, xs)
    end
```

### Combining functions
```sml
fun compose(f,g) = fn x => f(g x)

(* ('b -> 'c) * ('a -> 'b) -> ('a -> 'c) *)
f o g

fun sqrt_of_abs i = Math.sqrt (Real.fromInt (abs i))
fun sqrt_of_abs i = (Math.sqrt o Real.fromInt o abs) i
val = sqrt_of_abs = Math.sqrt o Real.fromInt o abs

(* |> !> *)
infix !>

fun x !> f = f x
fun sqrt_of_abs i = i !> abs !> Real.fromInt !> Math.sqrt

fun backup1 (f,g) = fn x => case f x of
								NONE => g x
							  | SOME => y

fun backup1 (f,g) = fn x => f x handle _ => g x
```
### Currying
```sml
(* old way to get the effect of multiple arguments *)
fun sorted3_tupled (x,y,z) = z >= y andalso y >= x

val t1 = sorted3_tupled (7,9,11)

(* new way: currying *)
val sorted3 = fn x => fn y => fn z => z >= y andalso y >= x

(* alternately: fun sorted3 x = fn y => fn z => z >= y andalso y >= x *)

val t2 = ((sorted3 7) 9) 11

(* syntactic sugar for calling curried functions: optional parentheses *)
val t3 = sorted3 7 9 11 

(* syntactic sugar for defining curried functions: space between arguments *)
fun sorted3_nicer x y z = z >= y andalso y >= x

(* more calls that work: *)
val t4 = sorted3_nicer 7 9 11
val t5 = ((sorted3_nicer 7) 9) 11

(* calls that do not work: cannot mix tupling and currying *)
(*val wrong1 = ((sorted3_tupled 7) 9) 11*)
(*val wrong2 = sorted3_tupled 7 9 11*)
(*val wrong3 = sorted3 (7,9,11)*)
(*val wrong4 = sorted3_nicer (7,9,11)*)

(* a more useful example *)
fun fold f acc xs = (* means fun fold f = fn acc => fn xs => *)
  case xs of
    []     => acc
  | x::xs' => fold f (f(acc,x)) xs'
(* Note: foldl in the ML standard library is very similar, but 
   the two arguments for the function f are in the opposite order. 
   The order is, naturally, a matter of taste.
*)

(* a call to curried fold: will improve this call next *)
fun sum xs = fold (fn (x,y) => x+y) 0 xs
```
### Partial Application
Call functions with fewer arguments
- we get back a closure waiting for the remaining argument
- called partial application
  - can be used with any curried function
```sml
fun sorted3 x y z = z >= y andalso y >= x

fun fold f acc xs = (* means fun fold f = fn acc => fn xs => *)
  case xs of
    []     => acc
  | x::xs' => fold f (f(acc,x)) xs'

(* If a curried function is applied to "too few" arguments, that just returns
   a closure, which is often useful -- a powerful idiom (no new semantics) *)

val is_nonnegative = sorted3 0 0

val sum = fold (fn (x,y) => x+y) 0

(* In fact, not doing this is often a harder-to-notice version of
   unnecessary function wrapping, as in these inferior versions *)

fun is_nonnegative_inferior x = sorted3 0 0 x

fun sum_inferior xs = fold (fn (x,y) => x+y) 0 xs

(* another example *)

fun range i j = if i > j then [] else i :: range (i+1) j

val countup  = range 1
(* countup 6 = [1,2,3,4,5,6] *)

fun countup_inferior x = range 1 x

(* common style is to curry higher-order functions with function arguments
   first to enable convenient partial application *)

fun exists predicate xs =
    case xs of
      [] => false
    | x::xs' => predicate x orelse exists predicate xs'

val no = exists (fn x => x=7) [4,11,23]

val hasZero = exists (fn x => x=0)

val incrementAll = List.map (fn x => x + 1)

(* library functions foldl, List.filter, etc. also generally curried: *)

val removeZeros = List.filter (fn x => x <> 0)

(* but if you get a strange message about "value restriction", just put back
   in the actually-necessary wrapping or an explicit non-polymorphic type *)

(* does not work for reasons we will not explain here (more later) *)
(* (only an issue will polymorphic functions) *)

(* val pairWithOne = List.map (fn x => (x,1)) *)

(* workarounds: *)
fun pairWithOne xs = List.map (fn x => (x,1)) xs

val pairWithOne : string list -> (string * int) list = List.map (fn x => (x,1))

(* this different function works fine because result is not polymorphic *)
val incrementAndPairWithOne = List.map (fn x => (x+1,1))
```
### Currying Wrapup
```sml
(* generic functions to switch how/whether currying is used *)
(* in each case, the type tells you a lot *)

fun curry f x y = f (x,y)

fun uncurry f (x,y) = f x y

fun other_curry1 f = fn x => fn y => f y x

fun other_curry2 f x y = f y x

(* example *)

(* tupled but we wish it were curried *)
fun range (i,j) = if i > j then [] else i :: range(i+1, j)

(* no problem *)
val countup = curry range 1

val xs = countup 7
```

### Mutable Ref
- Sometimes we need "update to state of world"
```sml
ref e
e1 := e2
!e 
val x = ref 42 

val y = ref 42 

val z = x

val _ = x := 43

val w = (!y) + (!z) (* 85 *)

(* x + 1 does not type-check *)
```

### Callbacks
Library takes functions to apply later when an event occurs like
- When a key is pressed, mouse moves
- When the program enters some state

A library may accept multiple callbacks
- need closures include private data with different types 
```sml
(* these two bindings would be internal (private) to the library *)
val cbs : (int -> unit) list ref = ref []
fun onEvent i =
   let fun loop fs =
        case fs of 
          [] => ()
        | f::fs' => (f i; loop fs')
    in loop (!cbs) end

(* clients call only this function (public interface to the library) *)
fun onKeyEvent f = cbs := f::(!cbs)

(* some clients where closures are essential
   notice different environments use bindings of different types
 *)
val timesPressed = ref 0
val _ = onKeyEvent (fn _ => timesPressed := (!timesPressed) + 1)

fun printIfPressed i =
    onKeyEvent (fn j => if i=j
                        then print ("you pressed " ^ Int.toString i ^ "\n")
                        else ())

val _ = printIfPressed 4
val _ = printIfPressed 11
val _ = printIfPressed 23
val _ = printIfPressed 4
```

#HtC1x

## Arbitrary Sized Data
- Before we worked with fixed-size data
- List Mechanisms
  - cons a two argument constructor
  - first access first element
  - rest access the rest
  - only use these 2 can recursively get all the elements
```rkt
empty ;; empty list

(define L1(cons "Flames" empty))				 ;a list of 1 element
(cons "Leafs" (cons "Flames" empty))			 ;a list of 2 elements
(cons (string-append "C" "anucks") empty )

(cons 10 (cons 9 (cons 10 empty)))			     'a list of 3 elements

(first L1) ; prodce "Flames"

(rest L1) 

(first (rest L2)) ;get second
(first (rest (rest  L2))) ;get third

(empty? empty)	;check if list is empty
```

#### List Data Definition
```rkt
;Information:
 UBC "UBC"
 McGill "McGill"
 Team Who Must Not Be Named "Team Who Must Not Be Named"

(cons "UBS" (cons "McGill" empty))

;; ListOfString is one of:
;;  - empty
;;  - (cons String ListOfString)
;; interp. a list of strings 
(define LOS1 empty)
(define LOS2 (cons "McGill" empty))
(define LOS3 (cons "UBC (cons "McGill" empty)))

(define (fn-for-los los)
	(cond [(empty? los) (...)]
		  [else
		   (... (first los)
				(fn-for-los (rest los)))]))

;; Template rules used:
;;  - one of: 2 cases
;;  - atomic distinct: empty
;;  - compound: (cons String ListOfString)
;;  - self-ref : rest los is ListOfString
```
#### Functions operate on lists
```rkt

;;; ListOfString -> Boolean
;; produce true if los include UBC
(check-expect (constains-ubc? empty) false)
(check-expect (constains-ubc? (cons "McGill empty) false))
(check-expect (constains-ubc? (cons "UBC empty) false))
(check-expect (constains-ubc? (cons "McGill (cons "UBC" empty) true)))
	
;(define (constains-ubc? los)) false) ;stub

(define (constains-ubc? los)
	(cond [(empty? los) false]
		  [else
		   (if (string=?  (first los) "UBC")
			   true
			   (contains-ubc? (rest los)))]))

;; Data definitions:

;; ListOfNumber is one of:
;;  - empty
;;  - (cons Number ListOfNumber)
;; interp. each number in the list is an owl weight in ounces
(define LON1 empty)
(define LON2 (cons 60 (cons 42 empty)))

(define (fn-for-lon lon)
 (cond [(empty? lon) (...)]
	   [else
	    (... (first lon)
			 (fn-for-lon (rest lon)))]))

;; Template rules used:
;;  - one of: 2 cases
;;  - atomic distinct: empty
;;  - compound:(cons Number ListOfNumber)
;;  - self-ref: (rest lon) is ListOfNumber

;; Functions
;; ListOfNumber -> Number
;; produce total weight of Owl in consumed list
(check-expect (sum empty) 0)
(check-expect (sum (cons 60 empty) (+ 60 0)))
(check-expect (sum (cons 60 (cons 42 empty))) (+ 60 (+ 42 0)))

(define (sum lon) 0) ;stub

(define (sum lon)
 (cond [(empty? lon) (0)]
	   [else
	    (+ (first lon)
			(sum (rest lon)))]))

;; produce total numbers of weight in consumed list 
;; ListOfNumber -> Natural
```
![Image of Lecture]
(https://i.imgur.com/oTJtCQB.png)
