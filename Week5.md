# XML
- Extensible Markup Language
- Tags des content instead of formating, like HTML use tag
- Basic constructs
  - Tagged elements
    - nested
  - Attributes
  - Text
<p align="center">
	<img src="https://i.imgur.com/O235m9t.png">
</p>
- "Well-Formed" XML
  - Single root element
  - Matched tags, proper nesting
  - Unique Attributes within elements
  - Can check using XML Parser
    - if passed, -> Parsed XML e.g DOM SAX
### Displaying XML
- Use rule based alng to translate to HTML
  - CSS or XSL

## DTDs
- Valid XML adhers to basic structural requirements
  - also adheres to content-specific specification
- DTD: Document Type Descriptor
  - Grammar-like specifiying elements, attributes, nesting, ordering, #occurrences
  - also special attribute types ID and IDREFS (pointers)

<p align="center">
	<img src="https://i.imgur.com/cBSI2kd.png">
</p>

### XML Schema (XSD)
- like DTDs, can specify elements ... also data types, keys, (typed) pointers, and more
- XSD is written in XML
- types
- key
  - in DTD, ID must to gloablly unique, XSD ok 
- keyref
  - similar to a typed pointer, need to point to a certain type of key element(Book/Author)
- occurence constraint
  - min or max occurence
- ID unique, IDREF can repeat, IDREFS: multiple IDEREF
```dtd
<!ELEMENT Course_Catalog (Department*)>

<!ELEMENT Department (Title, Course+, (Professor|Lecturer)+)>
<!ATTLIST Department Code ID #REQUIRED
					 Chair IDREF #REQUIRED
>

<!ELEMENT Professor (First_Name, Middle_Initial?, Last_Name)>
<!ATTLIST Professor InstrID ID #REQUIRED>
<!ELEMENT First_Name (#PCDATA)>
<!ELEMENT Last_Name (#PCDATA)>
<!ELEMENT Middle_Initial (#PCDATA)>

<!ELEMENT Lecturer (First_Name, Middle_Initial?, Last_Name)>
<!ATTLIST Lecturer InstrID ID #REQUIRED>

<!ELEMENT Course (Title, Description? )>
<!ATTLIST Course Number ID #REQUIRED
				 Instructors IDREFS  #REQUIRED
				 Prerequisites IDREFS #IMPLIED
				 Enrollment CDATA #IMPLIED
>
<!ELEMENT Title (#PCDATA)>
<!ELEMENT Description (#PCDATA|Courseref)*>
<!ELEMENT Courseref EMPTY>
<!ATTLIST Courseref Number IDREF #REQUIRED>
```

# HtDP
## Natural Numbers
- Primitive function in Racket add1 and sub1 are like cons and rest
- Implement Natural Number using well-formed self-referential data def
```rkt
;; NATURAL is one of:
;;  - empty
;;  - (cons "!" NATURAL)
;; interp. a natrual number, the number of "!" in the list is the number 
(define N0 empty)
(define N1 (cons "!" N0))
(define N2 (cons "!" N1))

;; These are the primitives that operate NATURAL:
(define (ZERO? n) (empty? n))		;Any -> Boolean
(define (ADD1 n) (cons "!" n))		;NATURAL -> NATURAL
(define (SUB1 n) (rest n))			;NATURAL[>0] -> NATURAL

(define (fn-for-NATURAL n)
  (cond [(ZERO? n) (...)]
		[else
		 (... n
			  (fn-for-NATURAL (SUB1 n))]))

;; NATURAL NATURAL -> NATURAL
;; produce a + b
(check-expect (ADD N2 N0) N2)
(check-expect (ADD N3 N0) N3)
(check-expect (ADD N3 N4) N7)

;(define (ADD a b) N0)

(define (ADD a b)
  (cond [(ZERO? b) a]
		[else
		 (ADD (ADD1 a) (SUB1 b))]))

;; NATURAL NATURAL -> NATURAL
;; produce a - b
(check-expect (SUB N2 N0) N2)
(check-expect (SUB N3 N0) N3)
(check-expect (SUB N4 N3) N1)

;(define (SUB a b) N0)

(define (SUB a b)
  (cond [(ZERO? b) a]
		[else
		 (SUB (SUB1 a) (SUB1 b))]))

```
### Function Composition
- Use function composition when a function must perform two or more distinct and complete operations on the consumed data
```
;; Constants:

(define BLANK (square 0 "solid" "white"))

;; Data definitions:

;; ListOfImage is one of:
;;  - empty
;;  - (cons Image ListOfImage)
;; interp. An arbitrary number of images
(define LOI1 empty)
(define LOI2 (cons (rectangle 10 20 "solid" "blue")
                   (cons (rectangle 20 30 "solid" "red")
                         empty)))
#;
(define (fn-for-loi loi)
  (cond [(empty? loi) (...)]
        [else
         (... (first loi)
              (fn-for-loi (rest loi)))]))

;; Functions:

;; ListOfImage -> Image
;; lay out images left to right in increasing order of size
;; sort images in increasing order of size and then lay them out left-to-right
(check-expect (arrange-images (cons (rectangle 10 20 "solid" "blue")
                                    (cons (rectangle 20 30 "solid" "red")
                                          empty)))
              (beside (rectangle 10 20 "solid" "blue")
                      (rectangle 20 30 "solid" "red")              
                      BLANK))
(check-expect (arrange-images (cons (rectangle 20 30 "solid" "red")
                                    (cons (rectangle 10 20 "solid" "blue")
                                          empty)))
              (beside (rectangle 10 20 "solid" "blue")
                      (rectangle 20 30 "solid" "red")              
                      BLANK))

;(define (arrange-images loi) BLANK) ;stub

(define (arrange-images loi)
  (layout-images (sort-images loi)))


;; ListOfImage -> Image
;; place images beside each other in order of list
(check-expect (layout-images empty) BLANK)
(check-expect (layout-images (cons (rectangle 10 20 "solid" "blue")
                                    (cons (rectangle 20 30 "solid" "red")
                                          empty)))
			   (beside (rectangle 10 20 "solid" "blue")
					   (rectangle 20 30 "solid" "red")
					   BLANK))

;(define (layout-images loi) BLANK)

(define (layout-images loi)
  (cond [(empty? loi) (BLANK)]
		[else
		 (beside (first loi)
				 (layout-images (rest loi)))]))
   



;; ListOfImage -> ListOfImage
;; sort images in increasing order of size 
;; !!!
(define (sort-images loi) loi)
```
### Operating on a list
- You might need to operate on the whole list but not the first 
  - would need a helper function to do that 
  - if shifting domain knowledge, use helper

# ProgramLangA
## Type Inference
- Determin types of bindings in order
- For each val or fun binding:
  - Analyze def for all necessary fact
  - E.g: if see x > 0, then x must have type int
  - Type error if no way for all facts to hold
- After that use type variable ('a) for any unconstrained types
  - unused argument can have any type 
- Value restriction
  - a variable-binding can have a polymorphic type only if  the exp is a variable or value
```sml
(* 
   f : T1 -> T2 [must be a function; all functions take one argument]
   x : T1 [must have type of f's argument]

   y : T3 
   z : T4 

   T1 = T3 * T4 [else pattern-match in val-binding doesn't type-check]
   T3 = int [because (abs y) where abs : int -> int]
   T4 = int [because add z to an int]
   So T1 = int * int
   So (abs y) + z : int, so let-expression : int, so body : int, so T2=int
   So f : int * int -> int
*)
fun f x = 
   let val (y,z) = x in
       (abs y) + z
   end

(*
   sum : T1 -> T2 [must be a function; all functions take one argument]
   xs : T1 [must have type of f's argument]

   x : T3 [pattern match against T3 list]
   xs' : T3 list [pattern match against T3 list]

   T1 = T3 list [else pattern-match on xs doesn't type-check]
   0 : int, so case-expresssion : int, so body : int, so T2=int
   T3 = int [because x : T3 and is argument to addition]
   T2 = int [because result of recursive call is argument to addition]
   sum xs' type-checks because xs' has type T3 list and T1 = T3 list
   case-expression type-checks because both branches have type int

   from T1 = T3 list and T3 = int, we know T1 = int list
   from that and T2 = int, we know f : int list -> int
*)
fun sum xs =
   case xs of
     [] => 0
   | x::xs' => x + (sum xs')

(*
   type inference proceeds exactly like for sum for most of it:
   broken_sum : T1 -> T2 [must be a function; all functions take one argument]
   xs : T1 [must have type of f's argument]

   x : T3 [pattern match against T3 list]
   xs' : T3 list [pattern match against T3 list]

   T1 = T3 list [else pattern-match on xs doesn't type-check]
   0 : int, so case-expresssion : int, so body : int, so T2=int
   T3 = int [because x : T3 and is argument to addition]
   T2 = int [because result of recursive call is argument to addition]


   but now to type-check (broken_sum x) we need T3 = T1 and T1 = T3 list,
   so we need T3 = T3 list, which is impossible for any T3.

   Note: The actual type-checker might gather facts in a different order and
   therefore report a different error, but it will report an error.
*)
fun broken_sum xs =
    case xs of
	[] => 0
      | x::xs' => x + (broken_sum x)

(*
compose : T1 * T2 -> T3
f : T1
g : T2
x : T4

from body of compose being a function, T3 = T4->T5 for some T4 and T5
from g being passed x, T2 = T4->T6 for some T6
from f being passed result of g, T1 = T6->T7 for some T7
from f being body of anonymous function, T7=T5

putting it all together:
  T1=T6->T5, T2=T4->T6, and T3=T4->T5
so compose: (T6->T5) * (T4->T6) -> (T4->T5)
now replace unconstrained types /consistently/ with type variables:
('a -> 'b) * ('c -> 'a) -> ('c -> 'b)
*)
fun compose (f,g) = fn x => f (g x)
```
## Mutual Recursion
- Allow f to call g and g to call f
  - use language features ```and```
- Useful for Finite-State machine
``` sml
(* an example of mutual recursion: a little "state machine" for
   deciding if a list of ints alternates between 1 and 2, not ending with a 1.
   This example is simple, but any finite state machine can be programmed via
   a set of mutally recursive functions for the states.
*)
fun match xs =
    let fun s_need_one xs =
	    case xs of
		[] => true
	      | 1::xs' => s_need_two xs'
	      | _ => false
	and s_need_two xs =
	    case xs of
		[] => false
	      | 2::xs' => s_need_one xs'
	      | _ => false
    in
	s_need_one xs
    end

(* mutual recursion works fine in ML provided you can put the functions
   "next to each other".  Otherwise there are workarounds...
 *)
datatype t1 = Foo of int | Bar of t2
and t2 = Baz of string | Quux of t1

fun no_zeros_or_empty_strings_t1 x =
    case x of
	Foo i => i <> 0
      | Bar y => no_zeros_or_empty_strings_t2 y
and no_zeros_or_empty_strings_t2 x =
    case x of
	Baz s => size s > 0
      | Quux y => no_zeros_or_empty_strings_t1 y

(* code above works fine.  this version works if you cannot put the functions
   next to each other for some reason (e.g., different modules) *)

fun no_zeros_or_empty_strings_t1_alternate(f,x) =
    case x of
	Foo i => i <> 0
      | Bar y => f y

fun no_zeros_or_empty_string_t2_alternate x =
    case x of
	Baz s => size s > 0
      | Quux y => no_zeros_or_empty_strings_t1_alternate(no_zeros_or_empty_string_t2_alternate,y)

## Modules
- used with signature to hide info, as in other language use private and public 
- Signature is a way to Implement abstract datatype in ML
  - Make types abstract like the below code 
  - Another way is deny binding exist by not showing them
```sml 
(* this signature hides gcd and reduce.  
That way clients cannot assume they exist or 
call them with unexpected inputs. *)
signature RATIONAL_A = 
sig
datatype rational = Frac of int * int | Whole of int
exception BadFrac
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end

(* the previous signature lets clients build 
 any value of type rational they
 want by exposing the Frac and Whole constructors.
 This makes it impossible to maintain invariants 
 about rationals, so we might have negative denominators,
 which some functions do not handle, 
 and print_rat may print a non-reduced fraction.  
 We fix this by making rational abstract. *)
signature RATIONAL_B =
sig
type rational (* type now abstract *)
exception BadFrac
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end
	
(* as a cute trick, it is actually okay to expose
   the Whole function since no value breaks
   our invariants, and different implementations
   can still implement Whole differently.
*)
signature RATIONAL_C =
sig
type rational (* type still abstract *)
exception BadFrac
val Whole : int -> rational 
   (* client knows only that Whole is a function *)
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end 

(* this structure provides a small library 
   for rational numbers -- same code as in previous 
   segment, but now we give it a signature *)
structure Rational1 :> RATIONAL_A (*or B or C*) = 
struct
```
### Signature Matching
- Signature need to more specific than struct on type 

### Equivalent implementations
- Key purepose of abstraction is to allow diff. implementations to be eq
  - client cant tell
  - so can improve later

```
(* this signature hides gcd and reduce.  
That way clients cannot assume they exist or 
call them with unexpected inputs. *)
signature RATIONAL_A = 
sig
datatype rational = Frac of int * int | Whole of int
exception BadFrac
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end

(* the previous signature lets clients build 
 any value of type rational they
 want by exposing the Frac and Whole constructors.
 This makes it impossible to maintain invariants 
 about rationals, so we might have negative denominators,
 which some functions do not handle, 
 and print_rat may print a non-reduced fraction.  
 We fix this by making rational abstract. *)
signature RATIONAL_B =
sig
type rational (* type now abstract *)
exception BadFrac
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end
	
(* as a cute trick, it is actually okay to expose
   the Whole function since no value breaks
   our invariants, and different implementations
   can still implement Whole differently.
*)
signature RATIONAL_C =
sig
type rational (* type still abstract *)
exception BadFrac
val Whole : int -> rational 
   (* client knows only that Whole is a function *)
val make_frac : int * int -> rational
val add : rational * rational -> rational
val toString : rational -> string
end 

(* this structure uses a different abstract type.  
   It does not even have signature RATIONAL_A.  
   For RATIONAL_C, we need a function Whole.  
*) 
structure Rational3 :> RATIONAL_B (* or C *)= 
struct 
   type rational = int * int
   exception BadFrac
	     
   fun make_frac (x,y) = 
       if y = 0
       then raise BadFrac
       else if y < 0
       then (~x,~y)
       else (x,y)

   fun Whole i = (i,1)

   fun add ((a,b),(c,d)) = (a*d + c*b, b*d)

   fun toString (x,y) =
       if x=0
       then "0"
       else
	   let fun gcd (x,y) =
		   if x=y
		   then x
		   else if x < y
		   then gcd(x,y-x)
		   else gcd(y,x)
	       val d = gcd (abs x,y)
	       val num = x div d
	       val denom = y div d
	   in
	       Int.toString num ^ (if denom=1 
				   then "" 
				   else "/" ^ (Int.toString denom))
	   end
end
```
### Equivalent Function
<p align="center">
	<img src="https://i.imgur.com/IYJsOi2.png">
	<img src="https://i.imgur.com/UXTtEvz.png">
</p>
