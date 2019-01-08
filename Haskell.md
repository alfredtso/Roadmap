### Algebric Type
- We can use record syntax to constuct, and we dont need to worry about order
- Haskell also made the Constrcutor function automatically
- A Val Constructor can take some val param to produce new val
	-  Type constructor can take type as param to produce new type
	- We should never add typeclass constraint in data declaration
		-  since we dont benefit a lot, since we will have to put contraints on function that operate on too.
		- When declaring a data type, the part before the = is the type constructor and the constructors after it (possibly separated by |'s) are value constructors
		- so `Vector t t t -> Vector t t t -> t` is wrong, since its not a type 
#### Derived Instances
- a type can be made an instance of a typeclass if it support that behaviours
- `Int` type is an instance of the `Eq` typeclass, `Eq` defines behaviours
- Typeclass are more like interface, we dont make data from typeclasses
	- instead, we first make data type and think about the behaviours
	- then we make it an instance of some typeclass
	- e.g if can be equated, we make it an instance of `Eq` or ordered `Ord`
#### Type Synonyms
- Can be parameterized
- `type AssocList k v = [(k, v)]`
- We can partially apply type parameters and get new type constructors
- also do not confuse type constructor and value constructor
