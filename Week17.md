
- Always use List<?> = new ArrayList<?> since you can change implementation without changing all the shit.
[SO](https://stackoverflow.com/questions/2279030/type-list-vs-type-arraylist-in-java)

### final keyword in java
- final variable
	-  to create constant variables
-  final methods
	-  prevent method overriding
-  final classes
	-  prevent inheritance

### Static initializer
- Can be used to initialize things cannot be done in one line or needed some other object 
[SE](https://softwareengineering.stackexchange.com/questions/228242/working-with-static-constructor-in-java)

### >>> shifting
- Because Java doesn't support unsigned int
- Adding 2 large sum can cause overflow resulting in incorrect result
```java
int mid = (low + high) >>> 1
int mid = (low + high)/2
```
### DateFormatter
```java
DateFormat dateFormat = new SimpleDateFormat("hh mm a");
```

### Calendar
- Abstract class vs Interface
- Interface only method signatures, Abstract class has implementation
	- Abstract class implement interface, no need to do all method
	- But subclass needs to implement all i.e the remaining method in interface

#### Paradigm
- Data Abstraction is the main organizing principle for building software
	- Explicit state allows to model change
		- Supports data abstraction too
- Concurrency is a property of systems that are made of activities that progess independently
	- **Deterministic dataflow** is a form of concurrency that always give s same outputs given same inputs

### State
- In FP, there is no notion of time
- Sometimes we will need the concept of time to model change in real 
- We need an abstract concept of time: change
	- e.g a state: a seq of values calculate progressively
- FP can observe implicit state too, but we need explicit ones
	- We will extend the language to enable that
	- **Cell** is a box with a content, box remain the same, content can change
		- a cell is a box with an identity and a content
			- the identity is a constant
			- the content is a variable

- Full store has 2 parts

- Static/Context Environ
- Dynamic environment refers to environment
- `val x = e``` ~ ```<x>:=<v>`
- in Haskell, list is also homogenous
	- ++ for putter two list tgt, but slow
	- : can be used to put it in front, [1,2,3] is just sugar for 1:2:3:[]
	- !! can used to get stuff with index 0 based
	- `head, tail, last, init, length, null, reverse, take, drop`
	- take 3 [5, 4, 3, 2, 1] => [5, 4, 3]
	- `elem` is called infix fx becuase ` 4 `elem` [3,4,5,6]`
	- range: [1..20], ['a'..'z'], [3,6..20] => [3,6,9,...], doesnt work with float
	- [13,26..] doesnt work because its lazy, instead do `take 10 (cycle [1,2,3])`
	- or `take 10 (repeat 5)`
	- list comprehension `[x*2 | x <- [1..10], x `mod` 7 == 3]`
	- e.g `length' xs = sum [1 | _ <- xs]`
	- `removeUpper st = [c | c <- st, c 'elem' ['A'..'Z']]`
	- `[ [ x | x <- xs, even x ] | xs <- xss]`

- Types in Haskell
	- use command :t 'a' to inspect type of 'a'
	- -> and * is haskell and ml way of separating param, not implying anythg
	- `Int` is bounded has max and min, `Integer` is unbounded so can big, slower
	- a is type variable and much like generics in other lang
	- Helps clarify thinking and express program structure
		- first step in writing in haskell is write down all the types, non-trivial
	- looking at fx type can tell you a lot about what the fx can be used, before reading
	- can find stuff based on the type you might expect

- Typeclass
	- Everything before => is called a class constraint
	- elem has a type of `(Eq a) => a -> [a] -> Bool` because it uses == over a list to check
	- Eq is used for types that support equality testing, the function its member (meaning the those have seen so far.. Int etc) are `==` and `/=` except function
	- `Ord` covers `< > >= <=`, same as `Eq` doesnt apply to function
	- `compare` takes two `Ord` member of same type and return `Ordering` which has `GT LT EQ`
	- `Show` can represent members as strings, `Read` opposite of `Show`
	- ` read "[3, 4]" ++ 3 => [3,4,3]` but `read "3"` error cause ghci cant infer the type of the result
	- `Enum` members (succ and pred) are sequentially order typed `Bounded` members have bounds
	- fromIntegral :: (Integral a, Num b) => a -> b, useful if we want `Integral` (Int, Integer) works with `Floating`
	- 

- Function application binds tighter than any binary operators.
- . in haskell works much like pipe in UNIX f (g x) = (f . g) x
- write from left and right easier to think, $ is like lambda
- `:info Num` check info about the typeclass
- pattern matching | is like a big if else tree but better style
- Where bindings are a syntactic construct that let you bind to variables at the end of a function and the whole function can see them, including all the guards. Let bindings let you bind to variables anywhere and are expressions themselves, but are very local, so they don't span across guards.
- `let <bindings> in <expression>` and let itself are expression, whereas bindings are syntactic constructs
- the type of the whole Let-expression is the type of e
- if statement is also expression so you can use them anywhere
- we can use ; to separate let statement
- case expression is almost like pattern matching but case expression can be used anywhere

```haskell
div_mod x y = (x `div` y, x `mod` y)

sort_pair x = if (fst x) < (snd x) then x else ((snd x), (fst x))

getFirstNthDigit x n = x `div` 10^n

numberofDigits :: Integer -> Integer
numberofDigits x = toInteger(round(logBase 10 (fromIntegral x)) + 1)

digs :: Integral x => x -> [x]
digs 0 = []
digs x = digs (x `div` 10) ++ [x `mod` 10]

firstTwoDigit :: Integer -> Integer
firstTwoDigit x
  | (length . show $ x) >= 2 = read . take 2 . show $ x
  | otherwise = error "less than 2 digits"

kMultiplication :: Integer -> Integer -> Integer
kMultiplication x y 
  |  len == 1 = x
  | otherwise = y
  where len = length (digs x)
      --(10^(digs x)) * kMultiplication 

first :: (a, b, c) -> a
first(x,_,_) = x

second :: (a, b, c) -> b
second(_,x,_) = x

third :: (a, b, c) -> c
third(_,_,x) = x

length' :: (Num b) => [a] -> b
length' [] = 0
length' (_:xs) = 1 + length' xs

bmiTell :: (RealFloat a) => a -> a -> String
bmiTell weight height
  | bmi <= skinny = "1"
  | bmi <= normal = "2"
  | bmi <= fat    = "3"
  where bmi = weight / height ^ 2
        (skinny, normal, fat) = (18.5, 20.5, 22.5)

initials :: String -> String -> String
initials firstname lastname = [f] ++ "." ++ [l]
  where (f:_) = firstname
        (l:_) = lastname

calBmis :: (RealFloat a) => [(a, a)] -> [a]
calBmis xs = [ bmi w h | (w, h) <- xs]
  where bmi weight height = weight / height ^ 2

calBmis' :: (RealFloat a) => [(a, a)] -> [a]
calBmis' xs = [ bmi | (w, h) <- xs, let bmi = w / h ^ 2]
-- We could do this like we did in list compre like predicate, except it doesnt filter
-- We omit the in because the visibility of the names is already predefined there
-- we can use let in bindings in a predicate but will only be visible to that predicate


cyclinder :: (RealFloat a) => a -> a -> a
cyclinder r h = 
    let sideArea = 2 * pi * r * h
        topArea = pi * r ^ 2
    in sideArea + 2 * topArea

describeList :: [a] -> String
describeList xs  = "The list is" ++ case xs of [] -> "empty"
                                               [x] -> "singleton"
                                               xs -> "a long list"

describeList' :: [a] -> String
describeList' xs = "The list is" ++ what xs
    where what [] = "empty"
          what [x] = "single"
          what (x:xs) = "long"
```

### Object-Oriented
- Robustness of Code
	- Stuffs might change the behaviours when scale up and run for long time
	- Exception and Assertion
### Exception
- Exception is thrown when the error pass through the stack
- try and catch block is to deal with the error
- Exception has class hierachy too, make use of this
