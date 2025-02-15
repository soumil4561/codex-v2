# Types of Relational Models:

1. Conceptual Model: describes the database at a very high level and is useful to understand the needs or requirements of the database. It is this model, that is used in the requirement-gathering process i.e. before the Database Designers start making a particular database. Popular eg: ER Model. Designed for a business audience.
2. **Representational Data Model:** This type of data model is used to represent only the logical part of the database and does not represent the physical structure of the database. The representational data model allows us to focus primarily, on the design part of the database. Popular eg: Relational Model. The advantage of using a Representational data model is to provide a foundation to form the base for the Physical model
3. **Physical Data Model:** The physical Data Model is used to practically implement Relational Data Model. It has all the information on the format in which the files are present and the structure of the databases, the presence of external data structures, and their relation to each other. Here, we basically save tables in memory so they can be accessed efficiently. SQL is a popular example for implementation.

Some other models:

1. Hierarchical Model: data are viewed as a collection of tables, or we can say segments that form a hierarchical relation. In this, the data is organized into a tree-like structure where each record consists of one parent record and many children. Even if the segments are connected as a chain-like structure by logical associations, then the instant structure can be a fan structure with multiple branches.
2. Network Model: This model is the generalization of the hierarchical model. This model can consist of multiple parent segments and these segments are grouped as levels but there exists a logical association between the segments belonging to any level.
3. Object Oriented Data Model: data and their relationships are contained in a single structure which is referred to as an object in this data model. In this, real-world problems are represented as objects with different attributes. All objects have multiple relationships between them.

  

# Entity-Relationship Model (ER Model)

The Entity Relational Model is a model for identifying entities to be represented in the database and representation of how those entities are related. The ER data model specifies enterprise schema that represents the overall logical structure of a database graphically.

## Symbols used in ER Model:

![[attachments/Untitled 2.webp|Untitled 2.webp]]

## Components of ER Model:

![[Untitled 1.webp]]

- Entity
    
    An Entity may be an object with a physical existence – a particular person, car, house, or employee – or it may be an object with a conceptual existence – a company, a job, or a university course.

    **Entity Set:** An Entity is an object of Entity Type and a set of all entities is called an entity set  
  
    
    Strong Entity: A type of entity that has a key Attribute. Does not depend on other Entity in the Schema. It has a primary key, that helps in identifying it uniquely, Represented by a rectangle.

    Weak Entity: An Entity type has a key attribute that uniquely identifies each entity in the entity set. But some entity type exists for which key attributes can’t be defined. Represented by Double Rectangle.

    Example: A company may store the information of dependents (Parents, Children, Spouse) of an Employee. But the dependents can’t exist without the employee. So Dependent will be a **Weak Entity Type** and Employee will be Identifying Entity type for Dependent, which means it is **Strong Entity Type**.

    ![[attachments/Untitled 6.png|Untitled 6.png]]
    
- Attributes
    
    Properties that define the entity type. Ex: Roll no, age, DOB define the Student Entity Type.
    
    1. Key Attribute: The attribute which **uniquely identifies each entity** in the entity set is called the key attribute. Eg: Roll no, employee id, etc. Key Attribute is represented by an oval with underlying lines.
        
        ![[attachments/Untitled 1 3.png|Untitled 1 3.png]]
        
    2. **Composite Attribute:** An attribute **composed of many other attributes** is called a composite attribute. Represented by Ovals connecting to a parent attribute.
    3. **Multivalued Attribute:** An attribute consisting of more than one value for a given entity. For example, Phone_No (can be more than one for a given student). Represented by Double Oval.
    
    ![[attachments/Untitled 2 3.png|Untitled 2 3.png]]
    
    1. Derived Attribute: An attribute that can be derived from other attributes of the entity type is known as a derived attribute. e.g.; Age (can be derived from DOB). Represented by dotted oval.
    
    ![[attachments/Untitled 3 2.png|Untitled 3 2.png]]
    
    The Complete Entity Type Student with its Attributes can be represented as:
    
    ![[attachments/Untitled 4 2.png|Untitled 4 2.png]]
    
      
    
- **Relationship**
    
    A Relationship Type represents the association between entity types. represented by a diamond and connecting the entities with lines.
    
    ![[attachments/Untitled 5 2.png|Untitled 5 2.png]]
    
    A set of relationships of the same type is known as a relationship set.
    
    Degree of Relationship:
    
    1. **Unary Relationship:** When there is only ONE entity set participating in a relation, the relationship is called a unary relationship. For example, one person is married to only one person. 
    2. Binary Relationship: When there are TWO entities set participating in a relationship, the relationship is called a binary relationship. For example, a Student is enrolled in a Course.
    3. **Ternary Relationship:** When there are n entities set participating in a relation, the relationship is called an n-ary relationship.
    
      
    
    Cardinality of Relationships: The number of times an entity of an entity set participates in a relationship set
    
    1. **One-to-One:** When each entity in each entity set can take part only once in the relationship, the cardinality is one-to-one.
        
        the total number of tables that can be used in this is 2.
        
    
    ![[attachments/Untitled 6 2.png|Untitled 6 2.png]]
    
    ![[Untitled 7.png]]
    
    1. **One-to-Many:** each entity can be related to more than one entity.
        
        total number of tables that can used is 3.
        
        ![[Untitled 8.png]]
        
    2. **Many-to-One:** When entities in one entity set can take part only once in the relationship set and entities in other entity sets can take part more than once in the relationship set  
        The total number of tables that can be used in this is 3.  
        
        ![[Untitled 9.png]]
        
    3. **Many-to-Many:** When entities in all entity sets can take part more than once in the relationship cardinality is many to many. Let us assume that a student can take more than one course and one course can be taken by many students.
        
        ![[Untitled 10.png]]
        

Participation Constraint: Applied to the entity participating in the relationship set.

1. **Total Participation –** Each entity in the entity set must participate in the relationship. shown by a double line
2. **Partial Participation –** The entity in the entity set may or may NOT participate in the relationship.  

![[Untitled 11.png]]

![[Untitled 12.png]]

## Steps to Draw ER Model:

3. Identify all Entities, place them in rectangles and label them.
4. Identify the relationship between them and place them accordingly using the Diamond. Make sure that Relationships are not connected to each other.
5. Attach attributes to the entities properly.
6. Remove redundant entities and relationships (if any).

  

## Enhanced ER Model

Enhanced entity-relationship diagrams are advanced database diagrams very similar to regular ER diagrams which represent the requirements and complexities of complex databases.

for displaying the Sub Class and Super Class; Specialization and Generalization; Union or Category; Aggregation etc.

  

### Specialization

an entity is divided into sub-entities based on its characteristics. It is a top-down approach where the higher-level entity is specialized into two or more lower-level [entities](https://www.geeksforgeeks.org/difference-between-entity-entity-set-and-entity-type/).  
Specialization is also called as ” Top-Down approch”.  

  

![[attachments/Untitled 2 2.webp|Untitled 2 2.webp]]

  

### Generalization

Generalization is the process of extracting common properties from a set of entities and creating a generalized entity from it. It is a bottom-up approach in which two or more entities can be generalized to a higher-level entity if they have some attributes in common.

Generalization is also called as ‘ Bottom-up approach”.

![[Untitled 3.webp]]

### Inheritence

Inheritance is a mechanism that allows subtypes to inherit attributes and relationships from their supertype. This means that any attribute or relationship defined for a supertype is automatically inherited by all its subtypes.

  

# Relational Model

Relations are Two-Dimensional Tables.

It uses the primary key and secondary key to connect any two files.

Normalization Theory is used to design the object-based data model.

[Relational Algebra](https://www.geeksforgeeks.org/introduction-of-relational-algebra-in-dbms/) and [Relational Calculus](https://www.geeksforgeeks.org/tuple-relational-calculus-trc-in-dbms/) are used to process the relations manually.  
  

### Jargon

A Relational Database Model consists of relations to connect them by key fields. A relation has some attributes. The relation is represented in rows and columns. Each column of the relation is called an attribute. Each row in the relation is called a tuple. Each relation can have one unique column i.e. primary key. Each relation can have n-columns and n-tuple. Each relation is preceded by the name of that relation. The fields of the relations are separated by commas and placed within the parentheses of the relation. The relational model represents data in the form of relations or tables.

```SQL
STUDENT (StudNo, Sname, Special)
ENROLLMENT (StudNo, Subcode, marks)
SUBJECT (Subcode, Subname, Maxmarks, Faccode)
FACULTY (Faccode, Fname, Dept) 
```

Example: Relational Model