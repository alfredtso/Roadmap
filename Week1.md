# 10/5/2018
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

#Singly-Linked Lists
  - Motivation
    - Array has limited flexibility, e.g if we need larger array on the go
	- we want to able to grow and shrink a collection of like values
  - comprised of node
    - data of some data type
	- a pointer to another node
	- ```
	typedef struct sllist
	{
	Value val;	
	struct sllist* next;
	} sllnode;
	```
	- although inside curly brace there is a self-ref, but you cant use sllnode
	- Operations
	  - create a linked list 
	    - ```sllnode* create(VALUE var);```
		- you need it to return a pointer to that linked list 
		  - ```sllnode* new = create(6)```
		  - dynamimcally allocate space for a new sllnode
		  - check to make sure mem
		  - initialize the ```val``` field
		  - initialize the ```next``` field with NULL
		  - return a pointer 'new'  to this node 
	  - search through a linked list to find an element
	    - you will want to keep track of the head node 
	    - ```bool find(sllnode* head, VALUE val);```
		  - create a traversal ptr pt to list head
		  - check if ```val``` is what we looking for
		  - if not, set the travesal ptr to the next ptr in the list 
		  - if reach the end, return fail
	  - insert new node
	    - ```sllnode* insert(sllnode* head, VALUE val);```
		- malloc for a new sllnode
		- check got mem
		- populate and insert the node at the begining of the linked list 
	      - can do it instantly, constant time complex
		- return a ptr to the new head of the linked list
	  - del a single element
	  - del an entire linked list 

