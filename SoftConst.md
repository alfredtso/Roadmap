### REQUIRES
```java
//REQUIRES amt >= 0
```
- to remind caller that sth to be satisfied if called
- preconditions for the method to run properly
### MODIFIES
- related to mutability
  - in java, varibale assignment will mutate the "object" pointed to
```java
//MODIFIES: this, deposit
```
- because we dont want to refer to the implementation
- refer to other objects too if modifies
- primitive data type are passed to function by copy
### EFFECTS
```java
//EFFECTS: adds amt to balance
//		   returns balance+amt
```
- Only talk about the effect, like adding them together, but not the implementation i.e: how do we add them
- Example
```java
//REQUIRES: nothing (or just dont write it at all
//MODIFIES: this
//EFFECTS: inserts num if not already there, if num is there, does nothing
public void insert (integer num){...}
```

