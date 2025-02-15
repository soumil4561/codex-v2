# Data, Database and File System

Data: collection of raw facts or figures that can be processed to derive meaning or knowledge.

Information: Data when processed becomes Information

Database: A database is a structured collection of interrelated data organised in a way that enables efficient storage, retrieval, and manipulation of information. It is a collection of interrelated data.

File System: A file system is a structure that an operating system uses to manage and organise files on a storage device. Disadvantages of file system:

- Data Redundancy
- Poor Memory Utilisation
- Data Inconsistency
- Data Security

## Database Management System

A Database Management System is software designed to manage, manipulate, and organise large volumes of data efficiently. It serves as an interface between the database and the users or applications, providing tools for storing, retrieving, updating, and managing data securely.

  

# Types of Database

- **Hierarchical Databases**
    
    Follows the progression of data being categorized in ranks or levels, wherein data is categorized based on a common point of linkage.
    
    due to such a structure, hierarchical databases are not easily salable; the addition of data elements requires a lengthy traversal through the database.
    
    ![[attachments/Untitled 2.png|Untitled 2.png]]
    
- **Network Databases**
    
    A network database is a hierarchical database, but with a major tweak. The child records are given the freedom to associate with multiple parent records.
    
    ![[attachments/Untitled 1 2.png|Untitled 1 2.png]]
    
    While a complex framework, network databases are more capable of representing two-directional relationships.
    
    Disadvantage lies in the inability to alter the structure due to its complexity and also in it being highly structurally dependent.
    
- Object-oriented databases
    
    Information stored in a database is capable of being represented as an object which response as an instance of the database model.
    
    the object can be referenced and called without any difficulty. As a result, the workload on the database is substantially reduced.
    
    Example: Berkeley DB software library
    
    ![[attachments/Untitled 2 2.png|Untitled 2 2.png]]
    
- Relational databases
    
    every piece of information has a relationship with every other piece of information. This is on account of every data value in the database having a unique identity in the form of a record.
    
    All data is tabulated in this model. Therefore, every row of data in the database is linked with another row using a primary key. Similarly, every table is linked with another table using a foreign key.
    
    Language used to interact with this Database: SQL
    
    ![[Untitled 3.png]]
    
- Cloud Database
    
    used where data requires a virtual environment for storing and executing over the cloud platforms and there are so many cloud computing services for accessing the data from the databases (like SaaS, Paas, etc).
    
- Centralized Database
    
    type of database that is stored, located as well as maintained at a single location.
    
- Operational Database
    
    It is used for creating, updating, and deleting the database in real-time and it is basically designed for executing and handling the daily data operation in organizations and businesses purposes.
    
- NoSQL databases
    
    A NoSQL database includes simplicity of design, simpler horizontal scaling to clusters of machines, and finer control over availability.
    
    Data structures used NoSQL databases are sometimes also viewed as more flexible than relational database tables.
    

# Applications of DBMS

![[Untitled 4.png]]

Examples:

|   |   |
|---|---|
|**Domain**|**Application**|
|E-commerce|E-commerce websites like Amazon, Flipkart, etc., have a DBMS of their  <br>customers. They have a track of orders sold, orders returned, defective  <br>pieces, etc.|
|Education|Schools have databases of their students. With the rise of online  <br>classes, physical attendance registers are being replaced by DBMS.|
|Reservation Systems|The Train Ticket Reservation System is one of the important examples.  <br>It has a database of lakhs and crores of customers. The information  <br>status can also be indicated as Waiting, confirmed, or RAC. Not to  <br>forget to mention that the waiting status also gets updated because of  <br>DBMS!|
|Manufacturing Industry|DBMS is used to keep records of all the details about the products like quantity, bills, purchase, supply chain management, etc.|

# Advantages/Disadvantages of DBMS

- Advantages
    
    - **Data Security**: By limiting the users and creating access policies for different users
    - **Data integration**: well-managed and synchronized forms of data, makes data-handling easy
    - **Data abstraction**
    - **Reduction in data Redundancy**
    - **Data consistency and accuracy: DBMS ensures that data is consistent and accurate by enforcing data integrity constraints and preventing data duplication**
    - **Efficient data access and retrieval: By indexing and query optimization techniques**
    - **Concurrency and maintained Atomicity: if some operation is performed on one particular table of the database, then the change must be reflected for the entire database.**
    - **Scalability and flexibility:** DBMS is highly scalable and can easily accommodate changes in data volumes and user requirements.
    
      
    
- Disadvantages
    
    - Increased Cost
        
        **Cost of Hardware and Software**
        
        **Cost of Staff Training**
        
        **Cost of Data Conversion**
        
    
    - **Complexity**
    - **Currency Maintenance**
    - **Performance**
    - **Frequency Upgrade**
    - **Security**  

# Data Abstraction

In order to make the system efficient in terms of retrieval of data, and reduce complexity in terms of usability of users, developers use abstraction i.e. hide irrelevant details from the users.

3 layers in data abstraction:

1. **Physical or Internal Level**: It tells us how the data is actually stored in memory. Access methods like sequential or random access and file organization methods like B+ trees and hashing are used for the same. If we store some data, blocks of storage, memory address and size are kept hidden from user.
2. **Logical or Conceptual Level**: This level comprises the information that is actually stored in the database in the form of tables. It also stores the relationship among the data entities in relatively simple structures. What logic is used to store the relations between different data?**
3. **View or External Level**: highest level of abstraction. Only a part of the actual database is viewed by the users.**

**Example:** In case of storing customer data,

- **Physical level** – it will contains block of storages (bytes,GB,TB,etc)
- **Logical level –** it will contain the fields and the attributes of data.
- **View level** – it works with [CLI](https://www.geeksforgeeks.org/linux-operating-system-cli-command-line-interface-and-gui-graphic-user-interface/) or [GUI](https://www.geeksforgeeks.org/difference-between-cli-and-gui/) access of database

![[Untitled 5.png]]

  

# DBMS Architecture

- Tier 1 Architecture
    
    In 1-Tier Architecture the database is directly available to the user, the user can directly sit on the DBMS and use it that is, the client, server, and Database are all present on the same machine.
    
- Tier 2 Architecture
    
    The 2-tier architecture is similar to a basic [client-server model](https://www.geeksforgeeks.org/client-server-model/). The application at the client end directly communicates with the database on the server side. APIs like ODBC and JDBC are used for this interaction.
    
    advantage of this type is that maintenance and understanding are easier, and compatible with existing systems. However, this model gives poor performance when there are a large number of users.
    
    Advantage: easy to access, scalable, more users can use simultaneously, etc.
    
- Tier 3 Architecture
    
    In [3-Tier Architecture](https://www.geeksforgeeks.org/introduction-of-3-tier-architecture-in-dbms-set-2/), there is another layer between the client and the server. The client does not directly communicate with the server. Instead, it interacts with an application server which further communicates with the database system and then the query processing and transaction management takes place. Useful in larger operations like running a large web application
    
    The 3-tier architecture divides an application’s components into three tiers or layers. Each layer has its own set of responsibilities.
    
    ![[Untitled.jpeg]]
    
      
      
    
    1. Physical Level: At the physical level, the information about the location of database objects in the data store is kept. physical level of a database describes how the data is being stored in secondary storage devices like disks and tapes and also gives insights on additional storage details.
    2. Conceptual Level: At conceptual level, data is represented in the form of various database tables. For Example, STUDENT database may contain STUDENT and COURSE tables which will be visible to users but users are unaware of their storage
    3. **External Level:**  An external level specifies a view of the data in terms of conceptual level tables. Each view is catered for a particular set of users. For Example, FACULTY of a university is interested in looking course details of students, STUDENTS are interested in looking at all details related to academics, accounts, courses and hostel details as well. So, different views can be generated for different users.
    
    **3 Tier Schema Architecture in DBMS:**
    
    1. Presentation Tier: The presentation tier is the user interface or client layer of the application. It is responsible for presenting data to the user and receiving input from the user.
    2. **Application Tier:** The application tier is the middle layer of the 3-tier architecture. This tier communicates with the presentation tier to receive user input and communicates with the data management tier to retrieve or store data. This tier may include application servers, web servers, or APIs.
    3. **Data Management Tier:** The data management tier is the bottom layer of the 3-tier architecture. It is responsible for managing and storing data. This tier can include databases, data warehouses, or data lakes.
    
      
    
    ![[Untitled.webp]]
    
      
    
    Benefits:
    
    Scalability: The architecture separates the application processing and data management layers, which allows for easy scalability of each layer independently.
    
    **Flexibility/Modularity:** The architecture allows for the replacement or upgrade of one layer without affecting the other layers.
    
    **Security:** The architecture provides an additional layer of security, as the data management tier can be isolated from the application and presentation tiers, reducing the risk of unauthorized access.