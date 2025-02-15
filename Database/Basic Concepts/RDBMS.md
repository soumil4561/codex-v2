## Keys

| **STUD_NO** | **SNAME** | **ADDRESS** | **PHONE** |
| ----------- | --------- | ----------- | --------- |
| 1           | Shyam     | Delhi       | 123456789 |
| 2           | Rakesh    | Kolkata     | 223365796 |
| 3           | Suraj     | Delhi       | 175468965 |

- Super Key
    
    The set of attributes that can uniquely identify a tuple is known as Super Key. A super key is a group of single or multiple keys that identifies rows in a table. It supports NULL values.
    
    - Adding zero or more attributes to the candidate key generates the super key.
    - A candidate key is a super key but vice versa is not true.
    - Super Key values may also be NULL  
          
        `STUD_NO+PHONE is a super key``**.**`
- Primary Key
    
    A primary key is **a column or a set of columns in a table whose values uniquely identify a row in the table**.
    
    - It is a unique key.
    - It can identify only one tuple (a record) at a time.
    - It has no duplicate values, it has unique values.
    - It cannot be NULL.
    - Primary keys are not necessarily to be a single column; more than one column can also be a primary key for a table.  
          
        `STUD_NO is a primary key`
    
      
    
- Candidate Key
    
    The minimal set of attributes that can uniquely identify a tuple is known as a candidate key.
    
    - It is a minimal super key.
    - It is a super key with no repeated data is called a candidate key.
    - The minimal set of attributes that can uniquely identify a record.
    - It must contain unique values.
    - It can contain NULL values.
    - Every table must have at least a single candidate key.
    - A table can have multiple candidate keys but only one primary key.
    - The value of the Candidate Key is unique and may be null for a tuple.
    - There can be more than one candidate key in a relationship.  
          
        `STUD_NO is the candidate key for relation STUDENT`

![[attachments/Untitled 2.jpeg|Untitled 2.jpeg]]

- Difference b/w primary and candidate key
    
    |   |   |   |
    |---|---|---|
    |S.NO|Primary Key|Candidate Key|
    |1.|Primary key is a minimal super key. So there is one and only one primary key in a relation.|While in a relation there can be more than one candidate key.|
    |2.|Any attribute of Primary key can not contain NULL value.|While in Candidate key any attribute can contain NULL value.|
    |3.|Its confirmed that a primary key is a candidate key.|But Its not confirmed that a candidate key can be a primary key.|  
    
- Alternate Key
    
    candidate key other than the primary key is called an [alternate key](https://www.geeksforgeeks.org/sql-alternate-key/)
    
    - All the keys which are not primary keys are called alternate keys.
    - It is a secondary key.
    - It contains two or more fields to identify two or more records.
    - These values are repeated.
    - Eg:- SNAME, and ADDRESS is Alternate keys
    
      
    

```Plain
Consider the table shown above.
STUD_NO, as well as PHONE both,
are candidate keys for relation STUDENT but
PHONE will be an alternate key
(only one out of many candidate keys).
```

![[attachments/Untitled 13.png|Untitled 13.png]]

- Foreign Key
    
    If an attribute can only take the values which are present as values of some other attribute, it will be a [foreign key](https://www.geeksforgeeks.org/foreign-key-constraint-in-sql/) to the attribute to which it refers. The relation which is being referenced is called referenced relation and the corresponding attribute is called referenced attribute. The referenced attribute of the referenced relation should be the primary key to it.
    
    - It is a key it acts as a primary key in one table and it acts as secondary key in another table.
    - It combines two or more relations (tables) at a time.
    - They act as a cross-reference between the tables.
    - Foreign Keys can be NULL
    - For example, DNO is a primary key in the DEPT table and a non-key in EMP
- Composite Key
    
    Sometimes, a table might not have a single column/attribute that uniquely identifies all the records of a table. To uniquely identify rows of a table, a combination of two or more columns/attributes can be used. It still can give duplicate values in rare cases
    
    - It acts as a primary key if there is no primary key in a table
    - Two or more attributes are used together to make a [composite key](https://www.geeksforgeeks.org/composite-key-in-sql/).
    - Different combinations of attributes may give different accuracy in terms of identifying the rows uniquely.
    
![[attachments/Untitled 1 4.png|Untitled 1 4.png]]


> [!important]  
> Number of super keys = 2^(n-k)where “n” is the number of attributes in the Relation R and “k” is the number of attributes in the Candidate Key Find Questions: https://www.geeksforgeeks.org/number-of-possible-superkeys-in-dbms/  

## Constraints

![[attachments/Untitled 2 4.png|Untitled 2 4.png]]


1. Domain Constraints: Every domain must contain atomic values(smallest indivisible units) which means composite and multi-valued attributes are not allowed.
2. **Key Constraints or Uniqueness Constraints:** A relation can have multiple keys or candidate  
    keys(minimal superkey), out of which we choose one of the keys as the primary key, we don’t have any restriction on choosing the primary key out of candidate keys, but it is suggested to go with the  
    [candidate key](https://www.geeksforgeeks.org/types-of-keys-in-relational-model-candidate-super-primary-alternate-and-foreign/) with less number of attributes. EACH KEY SHOULD BE UNIQUE.
3. **Entity Integrity Constraints:** Entity Integrity constraints say that no primary key can take a NULL value, since using the [primary key](https://www.geeksforgeeks.org/difference-between-primary-key-and-unique-key/) we identify each tuple uniquely in a relation.
4. **Referential Integrity Constraints:**

- The Referential integrity constraint is specified between two relations or tables and used to maintain the consistency among the tuples in two relations.
- This constraint is enforced through a foreign key, when an attribute in the foreign key of relation R1 has the same domain(s) as the primary key of relation R2, then the foreign key of R1 is said to reference or refer to the primary key of relation R2.
- The values of the foreign key in a tuple of relation R1 can either take the values of the primary key for some tuple in relation R2, or can take NULL values, but can’t be empty.

  

## Intension(Schema) and Extension(Instance)

1. Intension: Defines what kind of data can be stored and the relationship between them. Blueprint of the database. Doesn’t change frequently and is usually permanent for a database.
    
    Includes: Table definition (table names, columns, data types, etc.), Constraints, Relationship b/w tables(via foreign keys)
    
2. Extension: Actual data stored in the database at a given instance in time. Changes frequently.

## Anomalies in RDBMS

faults in the database caused due to poor management of storing everything in the flat database. It can be removed with the process of [Normalization](https://www.geeksforgeeks.org/normal-forms-in-dbms/), which generally splits the database which results in reducing the anomalies in the database.

1. Insertion Anomaly: If a tuple is inserted in referencing relation and referencing attribute value is not present in referenced attribute, it will not allow insertion in referencing relation.
2. Deletion/Updation: If a tuple is deleted or updated from referenced relation and the referenced attribute value is used by referencing attribute in referencing relation, it will not allow deleting the tuple from referenced relation.
    
    To avoid them, following can be used:
    
    **ON DELETE/UPDATE SET NULL:** delete/update the tuple from referenced relation and set the value of referencing attribute to NULL.
    
    **ON DELETE/UPDATE CASCADE:** delete/update the tuple from referenced relation and referencing relation as well.
    
## Relational Algebra

Language for querying relational data based on operators.

### Core Operators

1. Selection Operator**(σ):** Select tuples from a relation. Often based on some condition
    
    ![[attachments/Untitled 3 3.png|Untitled 3 3.png]]
    
2. Projection (**π):** Returns only chosen attributes/columns back. π is a list of attributes.
    
    By Default, projection removes duplicate data.
    
    ![[attachments/Untitled 4 3.png|Untitled 4 3.png]]
    
3. Cross product (X): Pairs rows from two tables. RxS: output is row rs for each row r and s, concat
    
    Cross product is commutative, i.e. for any R,S RxS = SxR.
    
4. Union (U): R and S should have same schema, it returns union of the 2 relations with duplicates removed.
5. Difference (-): R and S should have same schema, R-S returns all rows in R that are not in S.
6. Renaming (**ρ): “rename” a table and/or its columns.**
    
    ![[attachments/Untitled 5 3.png|Untitled 5 3.png]]
    
    No modification, it creates a copy. Useful if want to create natural join based on different named ids. etc.
    

### Derived Operators

1. Join: relate rows from two tables according to some criteria
    
    ![[attachments/Untitled 6 3.png|Untitled 6 3.png]]
    
    p is called join condition. for each row r in R and each row s in S, output a row rs if r and s satisfy p.
    
    Can be thought of as **σ(p) (R X S).**
    
2. Natural Join: relates rows from two tables and Enforce equality between identically named columns. Also, Eliminate one copy of identically named columns.
    
    Can be though of as **π (L) (R natural_join_p S). L is all columns with duplicates removed.**
    

![[attachments/Untitled 7 2.png|Untitled 7 2.png]]

1. Intersection: R and S should have same schema, Contains all rows that are in both R and S. Equivalent to (R - (R-S)) or (S - (S-R))

  

![[attachments/Untitled 8 2.png|Untitled 8 2.png]]


![[attachments/Untitled 9 2.png|Untitled 9 2.png]]