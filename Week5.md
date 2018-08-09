# XML
- Extensible Markup Language
- Tags des content instead of formating, like HTML use tag
- Basic constructs
  - Tagged elements
    - nested
  - Attributes
  - Text
<p align="center">
	<img src="https://i.imgur.com/O235m9t.png">
</p>
- "Well-Formed" XML
  - Single root element
  - Matched tags, proper nesting
  - Unique Attributes within elements
  - Can check using XML Parser
    - if passed, -> Parsed XML e.g DOM SAX
### Displaying XML
- Use rule based alng to translate to HTML
  - CSS or XSL

## DTDs
- Valid XML adhers to basic structural requirements
  - also adheres to content-specific specification
- DTD: Document Type Descriptor
  - Grammar-like specifiying elements, attributes, nesting, ordering, #occurrences
  - also special attribute types ID and IDREFS (pointers)
<p align="center">
	<img src="https://i.imgur.com/oTJtCQB.png">
</p>
### XML Schema (XSD)
- like DTDs, can specify elements ... also data types, keys, (typed) pointers, and more
- XSD is written in XML
- types
- key
  - in DTD, ID must to gloablly unique, XSD ok 
- keyref
  - similar to a typed pointer, need to point to a certain type of key element(Book/Author)
- occurence constraint
  - min or max occurence
- ID unique, IDREF can repeat, IDREFS: multiple IDEREF
```dtd
<!ELEMENT Course_Catalog (Department*)>

<!ELEMENT Department (Title, Course+, (Professor|Lecturer)+)>
<!ATTLIST Department Code ID #REQUIRED
					 Chair IDREF #REQUIRED
>

<!ELEMENT Professor (First_Name, Middle_Initial?, Last_Name)>
<!ATTLIST Professor InstrID ID #REQUIRED>
<!ELEMENT First_Name (#PCDATA)>
<!ELEMENT Last_Name (#PCDATA)>
<!ELEMENT Middle_Initial (#PCDATA)>

<!ELEMENT Lecturer (First_Name, Middle_Initial?, Last_Name)>
<!ATTLIST Lecturer InstrID ID #REQUIRED>

<!ELEMENT Course (Title, Description? )>
<!ATTLIST Course Number ID #REQUIRED
				 Instructors IDREFS  #REQUIRED
				 Prerequisites IDREFS #IMPLIED
				 Enrollment CDATA #IMPLIED
>
<!ELEMENT Title (#PCDATA)>
<!ELEMENT Description (#PCDATA|Courseref)*>
<!ELEMENT Courseref EMPTY>
<!ATTLIST Courseref Number IDREF #REQUIRED>
```

# HtDP
## Natural Numbers
- Primitive function in Racket add1 and sub1 are like cons and rest
- Implement Natural Number using well-formed self-referential data def
```rkt
;; NATURAL is one of:
;;  - empty
;;  - (cons "!" NATURAL)
;; interp. a natrual number, the number of "!" in the list is the number 
(define N0 empty)
(define N1 (cons "!" N0))
(define N2 (cons "!" N1))

;; These are the primitives that operate NATURAL:
(define (ZERO? n) (empty? n))		;Any -> Boolean
(define (ADD1 n) (cons "!" n))		;NATURAL -> NATURAL
(define (SUB1 n) (rest n))			;NATURAL[>0] -> NATURAL

(define (fn-for-NATURAL n)
  (cond [(ZERO? n) (...)]
		[else
		 (... n
			  (fn-for-NATURAL (SUB1 n))]))

;; NATURAL NATURAL -> NATURAL
;; produce a + b
(check-expect (ADD N2 N0) N2)
(check-expect (ADD N3 N0) N3)
(check-expect (ADD N3 N4) N7)

;(define (ADD a b) N0)

(define (ADD a b)
  (cond [(ZERO? b) a]
		[else
		 (ADD (ADD1 a) (SUB1 b))]))

;; NATURAL NATURAL -> NATURAL
;; produce a - b
(check-expect (SUB N2 N0) N2)
(check-expect (SUB N3 N0) N3)
(check-expect (SUB N4 N3) N1)

;(define (SUB a b) N0)

(define (SUB a b)
  (cond [(ZERO? b) a]
		[else
		 (SUB (SUB1 a) (SUB1 b))]))
```

