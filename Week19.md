# Design Principles
## SOLID
- Single Responsibility
	- each class should be centered around one cohesive concept
	- A symptom of having two responsibilities is having multiple cluster of methods
		- with each cluster refer to their own data within the class
- Coupling
	- Low: a change in one place requires no change in a collaborating class
	- Medium: a change in one class does required a remote change, but the compiler warns change is needed. eg when a checked exception is declared to be thrown, compiler will alert the developer that it must be caught at the calling loc. Method signature changes are also checked by the compiler, as are Type changes.
	-  High: a change in one class does require a remote change, but the collaboration will only be detected at runtime.
	- It's ok for a calss to have field from other class or calls method from other class
- Liskov Substitution
	- for subclass to be substitutable for its super, it cannot reduce the service it provides and cannot produce effect not produced in the superclass
	- So: A substitutable method can have wider/looser/weaker preconditions (wide top of smile), and narrower/tighter/stronger postconditions (narrow bottom of smile)
