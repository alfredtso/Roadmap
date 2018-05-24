# 10/5/2018
- [HtDP](#htdp)
- [6001x](#6001x)
- [ProgramLangA](#programlanga)
## CS50
- type struct example (node)
  - number
  - a pointer to the next node
- single linked list freed us from worry about mem space
- in for/while loop, we can have pointer and update pointer pointing to the next pointer
  - instead of int, and incrementing ++
- stack: Last In First Out
  - queue: First In First Out
# Binary serach tree
  - each node has two pointers
  - left point to another node(childer) which is smaller
  - right point to bigger
  - tree has one special root pointer
# hashing
  - separate a deck of card by suits
  - hashtable: array is not feasible since its linear
    - use array of pointers, linked list instead

# Structure
  - variable declaration
    - ```struct car \*mycar = malloc(sizeof(car));```
    - ```(\*mycat).year = 2010```
	- same as ```mycar->year = 2011;```
# Custom Data Types
  - ```typedef char\* string ```
  - combine struct and typedef, we have
    - ```typedef struct car {...} car_t;```
	- ```car_t mycar;```

# Singly-Linked Lists
  - Motivation
    - Array has limited flexibility, e.g if we need larger array on the go
	- we want to able to grow and shrink a collection of like values
  - comprised of node
    - data of some data type
	- a pointer to another node
```
typedef struct sllist
{
Value val;	
struct sllist* next;
} sllnode;
```
 
although inside curly brace there is a self-ref, but you cant use sllnode
## Operations
### create a linked list 

```sllnode* create(VALUE var);```

you need it to return a pointer to that linked list 

```sllnode* new = create(6)```

dynamimcally allocate space for a new sllnode
check to make sure mem
initialize the ```val``` field
initialize the ```next``` field with NULL
return a pointer 'new'  to this node 

### search through a linked list to find an element
- you will want to keep track of the head node 
```bool find(sllnode* head, VALUE val);```
- create a traversal ptr pt to list head
- check if ```val``` is what we looking for
- if not, set the travesal ptr to the next ptr in the list 
- if reach the end, return fail

### insert new node
```sllnode* insert(sllnode* head, VALUE val);```
- malloc for a new sllnode
- check got mem
- populate and insert the node at the begining of the linked list 
- can do it instantly, constant time complex
- return a ptr to the new head of the linked list
##### Order of manipulating ptr
- Set the new node next pointer to the old head
- Then we move the list head point to new node

### del a single element
- Can be messy since your need to go back one node and cant orphan the rest 
### del an entire linked list
```destroy(list);```
- If reach null ptr, stop (base-case in recursion)
- Del the rest of the list first.
- Free the current node

# Hashtable
- Combines the accessibility of array and dynamism of a linked list
  - Meaning if well-defined, insert del lookup in constant time
- To do so, create struct that when we insert data, data itself tell us where to find
  - tradeoff: not good at sorting or ordering
## hash function
- return non neg int val called hash code
- an array to store the data of type into the data struct

```C 
int x = hash('John'')
hashtable[x] = 'John'
```
## What makes a good hashfunction ?
- Use only the data being hashed
- Use all the data ...
- Deterministic, not heuristic
- Uniformly distribute data
- Generate very different hash code for similar data
```C
unsigned int hash(char* str)
{
	int sum = 0;
	for (int j =0; str[j] != '\0'; j++)
	{
		sum += str[j];
	}
	return sum % HASH_MAX;
}
```
- usually use source hashfunction

## Chaining
- An array of pointer pointing to different singly linked list

# Tries
Arrays and Hashtable are key-value data structure
- The data to be searched for is now a roadmap
- no two data (unless they are identical) have the same path
```C
typedef struct _trie
{
	char university[20];
	struct _trie* paths[10];
}
trie;
```
- University example
- Constant time insertion, deletion, lookup

# Stack
- statically declared mem, name variables etc
- function frame
- LIFO structure
- Operations
  - Push: add
  - Pop: remove
```C
typedef struct _stack
{
	VALUE array[CAP];
	int top;
}
stack;
```
## Push
- accept a pointer to the stack
- accept data of type VALUE to be added to the stack.
- add to the top of the stack
- change the location of the top
```void push(stack* s, VALUE data);```

## Pop
- Accept a ptr to the stack
- Change the location of the top
- Return the removed data to us
```VALUE pop(stack* s);```

## Linked-list implementation of Stacks
``` 
typedef struct _stack
{
	VALUE val;
	struct _stack* stack;
}
stack;
```
- always maintains a ptr to the head of the list
## Push 
- malloc a new node, set its next ptr to the head of the list, then move the head ptr to new
## Pop
- move to the second element and free the head, and move the head ptr to second elem
- move the trav pointer for the ptr to head to next element, remove the head
- move the head of list ptr to same as trav ptr

# Queues
- FIFO structure
- commonly implemented as array or linked list
## Operations
- Enqueue: Add new to the end
- Dequeue: remove the oldest from the front
```C
typedef struct _queue
{
	VALUE array[CAPACITY];
	int front;
	int size;
}
queue;
```
The front so we know which to dequeue

## Enqueue (push)
- same as push
- Except instead of add to top, this add to the end of the queue
- Change the size of the queue, whereas push change the top
``` void enqueue (*queue q, VALUE val); ```
- We dont want to change the front, which is also the oldest
- Instead we change the size 

## Dequeue (pop)
- Accept ptr ...
- Change the location of the front
- Decrese size
- Return value ...
``` VALUE dequeue(queue* q); ```

# Linked-list implementation of Queue
```C
typedef struct _queue
{
	VALUE val;
	struct _queue* next;
	struct _queue* prev;
}
queue;
```

## Enqueue
Make sure to always maintain not only head but also tail of the d-linked list
- malloc a new node; set the new next ptr to NULL, set its prev ptr to the tail
- set the tail's next to new; move tail ptr to the new node 

## Dequeue
- Same as pop
- Make new head prev ptr to Null

# Summary

Type | Insert, Deletion, Lookup | Sort | Memory | Downside
---- | ------------------------ | ---- | ------ | ---------
Array | Bad, Bad, Great | Relatively Easy | Relatively small | no flexibility
Linked List | Easy, easy, linear search | Difficult | not as small as array | 
Hashtable | Insertion: 2 Step, Easy, Average | Use array instead | >linked < Tries |
Tries | Ins: Complex, but its cont time. Easy, Fast | Sort as u build | Huge |

# 15/5/2018

## CS50 Pset5
### Speller
- dictionar.(c,h}
In dictionary.h, four fx declared, check what they do. In dictionary.c, reimplement fx.
so that it works fast.

- Makefile
A config file specify how to compile. might need to modify if want to include more file
should be to SRCS for .c files and HDRS for .h files

- speller.c
understand the code. ```getrusage``` will be benchmarking ``check load size unload```
You might want to provide speller with your own dictionary, since the one you implement
```load``` in dictionary.c might be dictionary/large

#### Specification
- Not alter ```speller.c```
- can alter dictionary.c, but no the declaratn of load check size unload
- can alter dictionary.h
- can alter makefile
- can add function to dictionary.c
- check must be case-insensitive and return boolean. e.g foo's is false even foo is true
- can assumre check only pass alphabet and apostrophes
- any dictionary passed to your program will be structured exactly like ours, lexicographically sorted from top to bottom with one word per line, each of which ends with \n.
- must not leak memory. Valgrind
#### Check
- if the word exits, can be found in hashtable
- which bucket will be in
- search the linked list 
```C
node *cursor = head;
whiel (cursor != NULL){
	// do sth
	cursor = cursor->next;
}
```
- traverse a trie
  - for each letter in input word
  - go to corres. element in children
    - if NULL, mispelled
	- if !NULL, move to next letter
  -once at the end, check if is_word is true

#### load
##### hashtable
- an array of buckets
- each bucket is a linked list
- Example
  - hashtable: 2 bucket, if (n%2 == 1), odd b, else even b. hashfunction
linked list implementation
```C
typedef struct node{
	// values the node holds, extra byte for pointer
	char word[LENGTH+1];
	// pointer to the next node
	struct node *next;
}

node *node1 = malloc(sizeof(node));
node *node2 = malloc(sizeof(node));
// since node 1 is a pointer, use -> to access
strcpy(node1->word , "Hello");
strcpy(node2->word , "World");

// point the next in node1 to node2
node1->next = node2;

// hashtable would be an array of node pointer
node *hashtable[50];
```
##### make a new word
- scan dict word by word
``` while (fscanf(file, "%s", word) != EOF)){...}```
- insdie the loop
  - malloc node
```C
node *new_node = malloc(sizeof(node));
if (new_node = NULL){
	unload();
	return false;
}
// if ok 
strcpy(new_node->word, word);
```
Inset into a linked list
```C
// point the new next to the node head pointing to
new_node->next = head;
head = new_node;
```

##### Hash Function
- take a string
- return an index less that the no. of buckets
- deterministic, i.e same item same index
- hash new_node->word
- insert into the linked list 

### tries
- every node contains an array of node ptr
  - one for every letter and '\''
```C
typedef	struct node{
	bool is_word;
	struct node *children[27];
}
node;
node *root;
```

### Unload
- linked list from head 
- tries from bottom, free all children, can try recursion too

## 16/5/2018
# 6001x
## Pylab
- import pylab as plt
``` plt.plot(mySamples, myLinear)```

```Python
plt.figure('linear')
plt.plot(X, Y)
plt.figure('Quad')
plt.plot(X, YSq)
plt.ylim(0,100)
plt.clf() #ipython seems to auto clf for you, maybe its because in diff cell, not sure
plt.plot(X, Y, label = 'linear')
plt.legend(loc='upper left')
plt.show()
```
This will allow you to plot on diff graph and can recall using the name
- plt.xlables, plt.title

### Visualizing
- A way to think about different kinds of result
  - Linear Vs Quadratic Vs Exponential as shown in lecture
  - Can use same scale to show diff
  - Or overlaying them over same plt.figure

#### Changing Display
- Line style
```plt.plot(X, Y, 'b-', label = 'linear')```
'b' stands for blue and '-' stands for dash
- Line Width
```plt.plot(X, Y, 'b-', label = 'linear', linewidth = 0.5)```
- Subplots
```plt.subplot(211) # meaning 2 rows 1 colums in pos 1```
- Changing Scales
``` plt.yscale('log')```

# HtDP
## Cond
- ```Cond``` is kind of like switch in C, multiplt cases
```
(define (function c)
  (cond [Q A]
        [Q A]))
```
## HtDD
- We might want to design data definition so that we can represent things in "Problem Domain" in "Program"
- For example, "the light is red" could be represented by 0 in the Program and in turn we can interpret the light using 0, 1, 2 in the program
#### Inherent Structure of the Information

- Atomic: cannot be taken apart into pieces that are meaningfully part of the same p. domain
```
;; CityName is String
;; interp. the name of a city //since its atomic, usually simple
(define CN1 "Boston")
(define CN2 "Hong Kong") 

(define (fn-for-city-name cn) 
  (... cn)) 

;;Template rules used:
;; - atomic non-distinct: String
;;
;;

;; Functions:

//Signature
;; CityName -> Boolean
//Purpose
;;produce true if the given city is the best in the world
(check-expect (best? "Boston") false)
(check-expect (best? "Hong Kong") true)

(define (best? cn) false) ;stub

; took template from CityName

(define (best? cn) 
  (if (string? cn "Hong Kong")
      true
	  false))

```
### Interval
- Represent a theater seat number with 32 seats each row
```
;; Seatnum is Integer[1, 32]
;; interp. the seat number in a row, 1 and 32 are aisle seats
(define SN1 1) ;aisle
(define SN2 12) ;random middle
(define SN3 32) ;aisle

(define (fn-for-sn sn)
  (... sn))

;; Template rules used:
;; - atomic non-distinct: Integer[1,32]

;; Functions;

;; SeatNum -> Boolean
;; produce true if the given seat number is on the aisle

(check-expect (aisle? 1) true)
(check-expect (aisle? 12) false)
(check-expect (aisle? 32) true)

; (define (aisle? sn) false) ;stub

; Use template from SeatNum

(define (aisle? sn)
  (or (= sn 1)
      (= sn 32)))
```

### Enumeration
- use enumeration when domain info. consist pf 2 or more distinct values 
- "one of", more than 2
```

;; LetterGrade is one of:
;;   - "A"
;;   - "B"
;;   - "C"
;; interp. the letter grade in a course
;; example are trivial for enumerations

(define (fn-for-letter-grade lg)
  (cond [(string=? lg "A") (...)]
        [(string=? lg "B") (...)]
		[(string=? lg "C") (...)]

;; Template rule used:
'' - one of: 3 cases 
;; - atomic distinct value: "A"
;; - atomic distinct value: "B"
;; - atomic distinct value: "C"

;; Functions:

;; LetterGrade -> LetterGrade
;; produce next highest letter grade (no change for A)
(check-expect (bump-up "A") "A")
(check-expect (bump-up "B") "A")
(check-expect (bump-up "C") "B")

; (define (bump-up lg) "A")

; Use template from LetterGrade
(define (bump-up lg)
  (cond [(string=? lg "A") ("A")]
        [(string=? lg "B") ("A")]
		[(string=? lg "C") ("B")]

```
- Fault injection
### Itermization
- with a *one of* but not all of the subclasses are single distinct value
- at least one class not single distinct value
```

;; CountDown is one of:
;; - false
;; - Natural[1,10]
;; - "complete"
;; interp.
;;     falese means countdown not yet started
;;     Natural[1,10 means countdown is running and hw many sec before midnight
;;     "complete" means countdown is over

(define CD1 false)
(define CD2 5)
(define CD3 "complete")


(define (fn-for-countdown cd)
  (cond [(false? cd) (...)]
        [(and (number? cd) (<= 1 c) (<= c 10))  (... cd)]
		[else (... )])



;; Template rule used:
;;  - one of: 3 cases
;;  - atomic distinct: false
;;  - atomic non-distinct: Natural[1,10]
;;  - atomic distinct: "complete"

;; Functions;

;; CountDown -> Image
;; produce image of current stage of CountDown
(check-expect (countdown-to-image false) (square "0" "solid" "white") )
(check-expect (countdown-to-image 5) (text (number->string 5) 24))
(check-expect (countdown-to-image "complete") (text "Happy New Year!!" 24 "red" ))

; (define (countdown-to-image c) (square 0 "solid" "white"))

; use template from CountDown

(define (countdown-to-image c)
  (cond [(false? c) 
         (square "0" "solid" "white")]
        [(and (number? cd) (<= 1 c) (<= c 10))  
		 (text (number->string 5) 24 "black")]
		[else 
		 (text "Happy New Year!!" 24 "red" )])

```
# 19/5/2018

## ProgramLangA
```sml

val x = 34;

val y = 17;
```
SML is a static environment meaning it type check
- All values are expressions but not vice versa
  - Every val "eval to itself in 'zero steps'

``` if e1 then e2 else e3 ```
- e2 and e3 can have any type, but must have same type t so is the entire exp.

#### Exercise
``` less-than ```
##### Syntax
``` e1 < e2 ```
##### Type-checking
- return type bool, e1 and e2 can be int in the same static env. 
##### Evaluation rules
- eval e1 to v1 and e2 to v2 in the same dynamic env, and produce true or false 

### Shadowing
- multiple variable bindings is often poor style 
```sml
val a = 10
val b = a * 2
val a = 5
val c = b
val d = a
val a = a + 1
```
There is no way to "assign to" a variable in ML (Python too, 10 is objecta and a is just a "pointer" to the object 

### Function
```sml
fun pow(x : int, y: int) = 
	if y=0
	then 1
	else x * pow(x,y-1)

fun cube(x : int) =
	pow(x, 3)

val sixtyfour = cube(4)
```
- Syntax
``` fun x0 (x1 : t1, ... , xn : tn) = e ```
- Evaluation
  - function is already a value !
  - Add x0 to env so later exp can call it
  - Func calls semantics also allow recursion
- Type-checking
  - Add binding x0 -> t if type-check body e in statis env containing:
    - "Enclosing" statis env
	- x1 : t1, ... , xn : tn
	- x0 : (t1 * ... * tn) -> t

### Pairs
- Syntax: (e1, e2)
- Evaluation: eval e1 to v1 and e2 to v2; result is (v1,v2)
- Type-checking: if e1 has type ta and e2 has type tb, then the exp has ta * tb
- Access:
  - SyntaxL: ```#1 e``` to access the first elem or e
  - Eval: eval to a pair of values, and return piece
  - Type-checking: If e has type ta*tb, then #1 e has type ta ... etc

### List
- Syntax: []
- Eval: "concat": 5::x; == add 5 into list x 
- Access:
  - Syntax: null e : checking if e is empty
  - if e eval to [v1,v2,...,vn], ```hd e``` eval to v1
  - ```tl e``` eval to [v2, ... , vn]

- t list is a list with all elements have type t
  - int list , bool list ... etc
- [] can have type t list for any t
  - SML use 'a list to denote such list
- For e1::e2 to type-checking, a t such that e1 has type t and e2 t list
  - ```null``` : 'a list -> bool
  - ```hd```: 'a list -> 'a
  - ```tl```: 'a list -> 'a list 

## List Functions
- Recursive example
```sml
fun sum_list (xs : int list) =
	if null xs
	then 0
	else hd xs + sum_list(tl xs)

fun countdown (x : int) = (* [7,6,5,4,3,2,1] *)
	if x = 0
	then []
	else x :: countdown(x-1)

fun append(xs : int list, ys : int list) =
	if null xs
	then ys
	else (hd xs) :: append((tl xs), ys)

(* (int list) * (int list ) -> in list *)

fun sum_pair_list (xs : (int * int) list) = 
	if null xs
	then 0
	else #1 (hd xs) + #2 (hd xs) + sum_pair_list(tl xs)

fun firsts (xs : (int * int) list) =
	if null xs
	then []
	else (#1 (hd xs)) :: firsts(tl xs)

fun seconds (xs : (int * int) list) =
	if null xs
	then []
	else (#2 (hd xs)) :: seconds(tl xs)

fun sum_pair_list2 (xs : (int * int) list) = 
	(sum_list(firsts xs)) + (sum_list(seconds xs))

```
- Function over lists are usu recursive
  - only way to get all the elements
- Whats the ans for empty list (base case)
- Whats the ans for non-empty
  - usu in terms of answer of tail of the list
- function produce list of potential any size will be recurs.
  - create list out of smaller lists 

### Let-expressions
Syntax
- ```let b1 b2 ... bn in e end ```
  - each bi is any binding and e is expression
Type-checking
- type-check each bi and e in a static environment that includ prev bindings
- type of the whole expression is the type of e
Eval
- eval each bi and e in a dynamic environment include prev binding
- result of the whole expression is the result of eval e
```sml
fun silly1 (z: int) =
	let
		val x = if z > 0 then z else 34
		val y = x + z + 9
	in
		if x > y then x * 2 else y * y
	end

fun silly2 () =
	let
		val x = 1
	in
		(let val x = 2 in x+1 end) + (let val y = x+2 in y+1 end)
	end
```
#### Scope
- where a binding is in the environment
  - in later bindings and the body of the let-expression
    - unless a later or nested bindings shadows it
  - Only in later bindings and body of the let-expression

#### Nested Function
```sml
fun countup_from1_better (x : int) = 
	let fun count (from : int) = 
			if from = x
			then x :: []
			else from :: count(from+1)
	in
		count 1
	end
```
Good style to define helper inside function they help if
- unlikely to be useful elsewhere
- likely to be misused elsewhere
- likely to be changed
Trade-off in code design: reusing code saves effort but makes reused code harder to change later

#### Let to avoid repeared computation
Bad code example
- bad as in each time recursive call twice -> exponential times many
```sml
fun bad_max(xs : int list) = 
	if null xs
	then 0
	else if null (tl xs)
	then hd xs
	else if hd xs > bad_max(tl xs)
	then hd xs
	else bad_max(tl xs)

fun good_max (xs : int list) =
    if null xs
    then 0
    else if null (tl xs)
    then hd xs
    else
	(* for style, could also use a let-binding for (hd xs) *)
	let val tl_ans = good_max(tl xs)
	in
	    if hd xs > tl_ans
	    then hd xs
	    else tl_ans
	end

fun countup(from : int, to : int) =
    if from=to
    then to::[]
    else from :: countup(from+1,to)

fun countdown(from : int, to : int) =
    if from=to
    then to::[]
    else from :: countdown(from-1,to)

```
#### Option
Motivation
- return 0 for empty list
  - raise an exception
- ```t option``` is a type for any type t
- Building:
  - NONE has 'a option
  - SOME e has type t option if e has type t
- Accessing:
  - isSome has type a option ->bool
  - valOf has type a option -> a (exception if given NONE)
```sml
(* better: returns an int option *)
fun max1 (xs : int list) =
    if null xs
    then NONE
    else 
	let val tl_ans = max1(tl xs)
	in if isSome tl_ans andalso valOf tl_ans > hd xs
	   then tl_ans
	   else SOME (hd xs)
	end

(* looks the same as max1 to clients; 
   implementation avoids valOf *)
fun max2 (xs : int list) =
    if null xs
    then NONE
    else let (* fine to assume argument nonempty because it is local *)
			fun max_nonempty (xs : int list) =
				if null (tl xs) (* xs better not be [] *)
				then hd xs
				else let val tl_ans = max_nonempty(tl xs)
					 in
						if hd xs > tl_ans
						then hd xs
						else tl_ans
					end
	in
	    SOME (max_nonempty xs)
	end
```
#### Booleans and Comparison
```sml
e1 andalso e2
e1 orelse e2
```
- e1 andalso e2 = if e1 then e2 else false
- e1 orelse e2 = if e1 then true else e2
- not e1 = if e1 then false else true
- e = if e then true else false

#### Benefit of Immutable Data
- read notes
- aliases will affect all the aliases if changed made to the object
- 5 things
  - Syntax: How to write constructs?
  - Semantics: What do porgram means ? Evaluation rules
  - Idoims: typical pattern for using lang feat. to express computation
  - Libraries
  - Tools: not actually part of lang, its something part implementation enable you
