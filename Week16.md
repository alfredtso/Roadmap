# Category
- A category consists of objects and arrows that go between them
### Composition
Must satisfy
1. Associative: h . (g . f) == (h . g) . f == h . g . f
2. For every object A there is an arrow which is a unit of composition. identity on A
- Name of concrete type starts with capital letters, names of type variables with lower
- Composition is the Essence of Programming
- Category theory is extreme in the sense that it actively discourages us from looking inside the objects. An object in category theory is an abstract nebulous entity. All you can ever know about it is how it relates to other objects — how it connects with them using arrows. This is how internet search engines rank web sites by analyzing incoming and out- going links (except when they cheat). In object-oriented programming, an idealized object is only visible through its abstract interface (pure surface, no volume), with methods playing the role of arrows. The mo- ment you have to dig into the implementation of the object in order to understand how to compose it with other objects, you’ve lost the ad- vantages of your programming paradigm.
### Who needs types
- dynamically typed language, type mismatches would be discovered at runtime, in strongly typed statically checked languages type mismatches are discovered at compile time, eliminating lots of in- correct programs before they have a chance to run
- As for the argument that unit testing can replace strong typing, consider the common refactoring practice in strongly typed languages: changing the type of an argument of a particular function. In a strongly typed language, it’s enough to modify the declaration of that function and then fix all the build breaks. In a weakly typed language, the fact that a function now expects different data cannot be propagated to call sites. Unit testing may catch some of the mismatches, but testing is al- most always a probabilistic rather than a deterministic process. Testing is a poor substitute for proof.

# SoftConstruction
### Overview
- Polymorphism
- Type
- Dispatch
#### Type
- There are three options to capture types
  1. Classes
    - Subclasses and Extends
		- inheritance gets all the methods in the superclass
```java
public class PrivatePlane extends Airplane {}

PrivatePlane privplane = new PrivatePlane();
privplane.fly();

PrivatePlane pp = new PrivatePlane();
pp.warmtowels(); // OK

Airplane luxPlane = new PrivatePlane();
luxPlane.warmtowels(); // Not OK

```
  - Method Overriding
	- to decide which method to use is method dispatch
		- which methods are allowed to call based on the declared type
		- but the implementation is used based on the actual type 
	- Not to be confused with method overLOADING
		- if same method is called, will depend on the type of the param
  - super();
	- can augment the subclass method using the super.superclassmethod()
  - Abstract Classes
	- good for capturing the generic stuff, default fields
	- it sits between a regular class and an interface
	- if an abstract class implements an interface, no need all the methods from
		the interface to be present, but all non-abstract subclass would need to
		implement the interface in full
	- 




  2. Interfaces
    - Interface defines the type and show the specification of Data Abstraction
    - can only do the public spec. 
	- contains sign of method but no implementation, and the list of methods all implementing types must fulfil
	- Classes (Abstract or Regular) can both implement interfaces
```java
InterfaceName thingy = new ClassName(); ClassNmae implements InterfaceName{..}
```
  - Interface is the declare type/apparent type; the instantiated type is the actual type
  - actual type can be a subclass or the same type as the apparent type
  - actual type means it is instantiated to be that type
  - apparent type means it is declared to be that type
  - a parameter declaration in amethod sign sets up the apparent type of the parameter
  - subtype Polymorphism
```java
public void touchAndGo(Flyer f){
	f.land();
	f.takeOff();
}

Bird birdie = new Bird();
Airplane plane = new Airplane();
```
  - Classes can implement multiple interfaces, and they have to provide for all
	  the implementation for all the method. 
  3. Abstract Classes
	- Abstract Class same but



