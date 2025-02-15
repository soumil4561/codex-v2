Database normalization is the process of organizing the attributes of the database to reduce or eliminate data redundancy.

It is the process of decomposing the relations into relations with fewer attributes.

Divide larger table into small tables and links them using relationships.

Eliminate anomalies like Insertion, Update and Deletion.

  

## Functional Dependencies

FD is a relationship that exists between two attributes. It typically exists between the primary key and non-key attribute within a table.

A functional dependency A->B in a relation holds if two tuples having the same value of attribute A also have the same value for attribute B.

Let X and Y be sets of attributes in a table T.

_**Y is functionally dependent on X iff,**_
for each set x belonging to X there is precisely one corresponding set y belonging to Y

  
  
_**Y is fully functional dependent on X if**_  
Y is functional dependent on X and  
Y is not functional dependent on any proper subset of X  

![[attachments/Untitled 14.png|Untitled 14.png]]

### Attribute Set

Attribute closure of an attribute set can be defined as set of attributes which can be functionally determined from it.

![[attachments/Untitled 1 5.png|Untitled 1 5.png]]

- If attribute closure of an attribute set contains all attributes of relation, the attribute set will be super key of the relation.
- If no subset of this attribute set can functionally determine all attributes of the relation, the set will be candidate key as well. For Example, using FD set of table 1,

### Prime vs Non Prime attribute

Attributes which are parts of any candidate key of relation are called as prime attribute, others are non-prime attributes. For Example, STUD_NO in STUDENT relation is prime attribute, others are non-prime attribute.

## Normal Forms

1. ***First Normal Form:*** 1NF: A table is in 1NF, if each row contains only atomic values(only one).
    
    ![[attachments/Untitled 2 5.png|Untitled 2 5.png]]
    
2. ***Second Normal Form:*** 2NF: A table is in 2NF, if:
    
    1. its in 1NF,
    2. No partial, only full dependency
    
    i.e. no non-prime attribute is dependent on any proper subset of any candidate key of the table (no partial, only full dependency, AB->C but not A->C or B->C, ... )
    
    Consider a book request table:
    
    ![[attachments/Untitled 3 4.png|Untitled 3 4.png]]
    
    Candidate Key is {BookNo, Patron}. But PhoneNo is only dependent the patron, rather than the Key, this makes this partial dependency. Solution: Put PhoneNo in separate Patron table
    
    ![[attachments/Untitled 4 4.png|Untitled 4 4.png]]
    
3. ***Third Normal Form***: 3NF: A table is in 3NF iff,
    
    1. its in 2NF,
    2. No transitive dependency,
    
    all its attributes are determined only by its candidate keys and not by any non-prime attributes
    
    i.e. for each dependency, LHS is candidate/super key or RHS is prime attribute.
    
    ![[attachments/Untitled 5 4.png|Untitled 5 4.png]]
    
    Candidate Key: BookNo. But Patron→ Address. Solution: Put address in separate Patron table
    
    ![[attachments/Untitled 6 4.png|Untitled 6 4.png]]
    
4. ***Boyce-Codd Normal Form:*** BCNF: Stronger Form of 3NF. A table is in BCNF if:
    
    1. It is in 3NF
    2. LHS must be Candidate key or Super Key for every one of its non-trivial dependencies X → Y, X is a super key for T
    
    - Trivial dependency: X → Y, Y is a subset of X
    - Most tables that are in 3NF also are in BCNF
    
    X should be a superkey for every functional dependency (FD) X−>Y in a given relation.
5. **Multivalued Dependency:** Consider a relation (table) `R` with attributes `A`, `B`, and `C`. We say there is a multivalued dependency from `A` to `B` (denoted as `A ->> B`) if:
	- For each value of `A`, there exists a set of values for `B`, independent of the values of any other attributes (such as `C`).

| Student | Hobby  | Course  |
| ------- | ------ | ------- |
| John    | Chess  | Math    |
| John    | Tennis | Math    |
| John    | Chess  | Physics |
| John    | Tennis | Physics |
	Here, `Student ->> Course` and `Student ->> Hobby` are multivalued dependencies because:

	- Each student is associated with multiple courses.
	- Each student is associated with multiple hobbies.
	- The set of courses a student takes is independent of their hobbies, and vice versa.
6. ***Fourth Normal Form:***  A table is in 4NF iff
	• In BCNF
	• No (or only one) multivalued dependency
		 For every non-trivial multivalued dependencies X => Y, X is either:
		 - A candidate Key
		 - A superset or a candidate Key
	i.e X is Unique
	![[Pasted image 20240831142523.png]]
	![[Pasted image 20240831142615.png]]

7. ***Fifth Normal Form:*** A table T is in 5NF iff Every non-trivial join dependency in it is implied by its candidate keys. 
	*A table T has join dependency if it can always be recreated by joining multiple tables each having a subset of the attributes of T*
	