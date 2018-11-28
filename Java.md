### "final" keywords ?
- [Answer from SO](https://stackoverflow.com/questions/15655012/how-does-the-final-keyword-in-java-work-i-can-still-modify-an-object)
### Array vesus linked-list 
[Answer from SO1](https://stackoverflow.com/questions/322715/when-to-use-linkedlist-over-arraylist-in-java)
[Answer from SO2](https://stackoverflow.com/questions/166884/array-versus-linked-list?rq=1)

### Static methods
- if the method is not using any instance variable
- not dependent on instance creation

### Nested classes
- allow you to decline a class within another class
	- Static nested
	- non-Static nested (inner class)
- Motivation
	- Consider, List and NOde used to implement a list
		-  List needs access to members value and next to Node
		-  Ideally, we want to define value and next to private
			-  but will prevent List from accessing
		-  Solve by nested class of List
-  Behave exactly like ordinary class but accessing with enclosing class
-  i.e "OutClass.StaticNestedClass
- Relationship of a **static** nested class with the outer class
	- nested class can access the privates and vice versa
	- method inside static nested can only access static members in the outer
[Some Example and Explanation](http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/8-List/nested-class.html)

