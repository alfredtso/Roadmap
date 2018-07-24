# Week3
- [CS50](#cs50)
- [RelationalAlgebra](#relationalalgebra)
- [ProgramLangA](#programlanga)
# CS50
- HTTP is a protocol
  - just like people greeting each other
- Browser send a envelope like "GET /HTTP/1.1" to Host: www.facebook.com
  - Then the server will send back "HTTP/1.1 404 Not Found"
- curl
- nslookup
- traceroute
- DHCP
- TCP
  - Ports: eg 80 for HTTP, 443 for HTTPS, 22 SSH
- TCP/IP
- CSS 
# ProgramLangA
### Record
- Just like dictionary in Python
  - Has field name (key) and value
- Its a each of type, meaning each 'entry' can have their own type t
  - Similar to tuple, where tuples are 'by-position', its by name
    - such mehtod is simpler for small no. of components, and too difficult for larger
- We can define tuples in terms of record using '1' '2' etc for field names
  - And type ``` t1 * ... * tn ``` is just ```{1:t1, ..., n:tn}```
  - This is what we call syntactic sugar
### Datatype
- We can build our own "one-of" types with ```dataype```
```sml
datatype mytype = TwoInts of int * int
				| Str of string
				| Pizza
```
- TwoInts ... etc we will call *contructors*
- 4 things will be added to the env
  - a new type ```mytype``` and three contructors
- Contructor
  - TwoInts is a function of type int*int -> mytype
- For list we access the values by using hd tl valOf etc
  - for datatype we dont need to, we can use case expressions
```sml
fun f x = (* f has type mytype -> int *)
	case x of
		Pizza => 3
	  | TwoInts(i1,i2) => i1 + i2
	  | Str s => String.size s
```
First eval e between case and of and second the expression in the *first branch that matches*
- We can have one branch for each variant of our datatype
- Each branch's expression must have same type
#### Pattern-matching
- Each pattern uses a diff const and pattern matching picks the branch with the right one
- This take care of the checking and getting out the underlaying data
- Case-expression is better than using hd like functions
  - never mess up
  - compiler wil check if you missed a branch
  - you can still write your own hd tl
- Example of one-of type
```sml
datatype id = StudentNum of int
			| Name of string * (string option) * string
```
```sml
datatype t = C1 of t1 | C2 of t2 | ... | Cn of tn
```
introduce a new type t and each constructor Ci is a function of type ti->t
To get at the pieces:
  - ```case e of p1 => e1 | p2 => e2 | ... | pn => en```
    - case expression eval e to a value v, find the pi matches v and eval ei to be the result of the whole case expression

### Type Synonyms
- ```type foo = int``` then we can use foo and int interchangeably 
  - Better use is ```type card = suit * rank```

### Polymorphic Datatypes
- Built-in lists and options are polymorphic
- Options are *exactly* pre-defined like this
``` datatype 'a option = NONE | SOME of 'a ```

### Pattern-Matching for Each-Of Types
- We have used pattern-matching for one-of types, can use it for each-of types 

# Relational Algebra
- Simplest query: relation name(table)
- Use operators to filter, slice, combine
  - Core: Relation, Select, Project, Cross Product, Union, Diff, Rename
  - Abbreviation: can be reproduced using core
    - Natural join, theta join, Intersection
``` 
\project_{pizza} ((\select_{gender = 'female' and age > 20} person) \join eats);
\project_{name}((\select_{pizzeria='Straw Hat'} serves) \join (\select_{gender='female'} (person \join eats)));
\project_{pizzeria}(\select_{price<10} serves \join \select_{name='Amy' or name='Fay'} eats);
\project_{pizzeria}(\select_{price<10} serves \join \select_{(n1='Amy' and n2='Fay') or (n1='Fay' and n2='Amy')}(\rename_{n1, pizza} eats \join \rename_{n2, pizza} eats));
\project_{name}(\select_{pizzeria='Dominos'} serves \join eats)
\diff
\project_{name}(\select_{pizzeria = 'Dominos'} frequents); 

(\project_{pizza} serves \diff \project_{pizza}(\select_{price > 10} serves))
\union 
(\project_{pizza} eats \diff \project_{pizza}(\select_{age >= 24}(eats \join person)));

(\project_{name, age}(\select_{pizza = 'mushroom'}Eats) \join Person))
\project_{age}(\select_{pizza = 'mushroom'} eats \join person)
\diff
\project_{age}
(\select_{a2 > age}
((\rename_{a2}
\project_{age}(\select_{pizza = 'mushroom'} eats \join person))
\join
\project_{age}(\select_{pizza = 'mushroom'} eats \join person)))

\project_{pizzeria} serves
\diff
\project_{pizzeria}(
serves
\join
((\project_{pizza} serves)
\diff
\project_{pizza}(
(\select_{age > 30} person) \join eats)))

(\project_{pizzeria}Serves) 
\diff 
    (\project_{pizzeria}((\project_{pizzeria}Serves) 
        \cross 
        (\project_{pizza}(\select_{age>'30'}Person \join Eats)) 
    \diff 
    (\project_{pizzeria,pizza}
    ((\select_{age>'30'}Person \join Eats) \join Serves))))
```

