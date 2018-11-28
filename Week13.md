# HtCx2
### Graph
- Different from arbitraty-arity tree in 2 ways
- can have cyclic
- have zero or multuple arrow pointing into a single node 
- cyclical graph doesnt show up in text well, usually quite messy

# Algorithm
### Master method
- number of subproblem in level j times the workdone for each subproblem
- the 3 cases:
- if the subproblem growth rate cancel out the shrinking rate of the input size, the total work is equal to number of work times number of level
- if the subprobem rate < shrinking rate, the total work is bounded upper by base case
- if the subproblem rate > shrinking, total work is bounded by the number of leaves
![master](https://i.imgur.com/AcSWbuZ.png)
![master1](https://i.imgur.com/Ysvt6Nd.png)
### Readings
- Subproblem not always constrained to being a constant fraction of original size
- e.g recursive version of linear search would create just one element less
# Database
### SQL
- Subqueries in WHERE clause
- Subqueries in SELECT,FROM clause
#### JOIN operators
- Inner join on condition
- Natural join: equate columns across tables of the same name, eliminate duplicate column
- Inner Join using (attrs)
- Left | Right | Full Outer join
- doesnt add expressive power, but some of them are actually quite complicated to construct
- explicitly use "join xxx using (xxx)" is better practice
- since natural join implicitly combines columns that have the same name and relation can have many many columns, you might end up equating some columns you didnt intend to
- full outer join is union of left outer join and right outer join
- commutative: most ops except right or left join
- associative: outer join is not 
#### Aggregation
- "min max avg count " in "select"
- group by
- "having" applys conditions on the group whereas "where" applys conditions on one tuple
#### Modifications
- "insert into R values(A1, A2... )"
- "delete from R where ... "
- "Update R Set Attr = Expression Where ..."
# ProgramLang
### Subtyping
- can solve problem in the lecture with record sometimes haveing extra field function dont need
- any value of subtype needs all fields any value of supertype has 
  - four rules in this example:
  - width subtyping
  - permutation
  - transitivity: if t1 is sub of t2 and t2 is that of t3 then t1 is sub of t3
  - reflexibility: every type is a subtype of itself
  - we need to have depth subtyping too, no because it breaks soundness, unless its immutable
#### Function Subtyping
- Question: when is one func type a subtype of another?
  - important for higher-order functions
  - also imp for understanding methods
  - if ta is sub of tb then t-> ta is sub of t -> tb
    - a function can return "more than it needs to"
	- return types are covariant
  - but we cannot allow argument subtyping, but allow backard subtype !
	- function can assume "less than it needs to" about arguments 
    - its called contravariant
- Classes Vs Types
  - A class defines an objects behaviour
  - subclassing inherits behaviour and cahnges it via extension and overriding
  - A Type describe an object's method's arguement/result types
  - A subtype is substitutable in terms of its fields/method types
  - !!! optional stuff to catch up
- Generic Vs Subtyping 
- Generic
  - parametric polymorphism 'a -> 'b
  - generally: when types can "be anything" but multiple things need to be "the same type"
- Subtyping
  - Code that "need a foo" but fine to have "more than a Foo"
  - Geometry on points in the example coloured point works well
  - GUI widegets specialize the basic idea of "being on the screen" and "responding to user actions"
### Benefits of No Mutation
- Can freely alias or copy values
- More function/modules are equivalent
- No local copies
- Depth subtyping is sound
