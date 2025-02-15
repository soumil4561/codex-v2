
Computer language used to interact with relational database systems. SQL is a tool for organizing, managing, and retrieving archived data from a computer database.
## SQL Commands

### DDL ( Data definition language )

commands used to create the database structure are known as data definition language.

|   |   |   |
|---|---|---|
|Command|Description|Syntax|
|[CREATE](https://www.geeksforgeeks.org/sql-create)|Create database or its objects (table, index, function, views, store procedure, and triggers)|`CREATE TABLE table_name (column1 data_type, column2 data_type, ...);`|
|[DROP](https://www.geeksforgeeks.org/sql-drop-truncate)|Delete objects from the database|`DROP TABLE table_name;`|
|[ALTER](https://www.geeksforgeeks.org/sql-alter-add-drop-modify)|Alter the structure of the database|`ALTER TABLE table_name ADD COLUMN column_name data_type;`|
|[TRUNCATE](https://www.geeksforgeeks.org/sql-drop-truncate)|Remove all records from a table, including all spaces allocated for the records are removed|`TRUNCATE TABLE table_name;`|
|[COMMENT](https://www.geeksforgeeks.org/sql-comments)|Add comments to the data dictionary|`COMMENT 'comment_text' ON TABLE table_name;`|
|[RENAME](https://www.geeksforgeeks.org/sql-alter-rename)|Rename an object existing in the database|`RENAME TABLE old_table_name TO new_table_name;`|

### DML (Data Manipulation Language )

A relational database can be updated with new data using data manipulation language (DML) statements.

|   |   |
|---|---|
|Command|Description|
|SELECT|Retrieves certain records from one or more tables.|
|INSERT|Creates a record.|
|UPDATE|Modifies records.|
|DELETE|Deletes records.|

### **DQL ( Data Query Language )**

Data retrieval instructions are written in the data query language (DQL), which is used to access relational databases. The SELECT command is used by software programs to filter and return particular results from a SQL table.

|   |   |   |
|---|---|---|
|**[SELECT](https://www.geeksforgeeks.org/sql-select-clause)**|It is used to retrieve data from the database|`SELECT column1, column2, ...FROM table_name<br>WHERE condition;`|

### DCL (Data Control Language )

used by database administrators to control or grant other users access to databases.

|   |   |
|---|---|
|Command|Description|
|GRANT|Gives a privilege to the user.|
|REVOKE|Takes back privileges granted by the user.|

### **TCL (Transaction Control Language)**

Transactions group a set of tasks into a single execution unit. Each transaction begins with a specific task and ends when all the tasks in the group are successfully completed.

Therefore, a transaction has only two results: success or failure.

|   |   |   |
|---|---|---|
|[BEGIN TRANSACTION](https://www.geeksforgeeks.org/sql-transactions/#:~:text=)|Starts a new transaction|`BEGIN TRANSACTION [transaction_name];`|
|[COMMIT](https://www.geeksforgeeks.org/sql-transactions/#:~:text=)|Saves all changes made during the transaction|`COMMIT;`|
|[ROLLBACK](https://www.geeksforgeeks.org/sql-transactions/#:~:text=)|Undoes all changes made during the transaction|`ROLLBACK;`|
|[SAVEPOINT](https://www.geeksforgeeks.org/sql-transactions/#:~:text=)|Creates a savepoint within the current transaction|`SAVEPOINT savepoint_name;`|

  

**Rename a database:**

ALTER DATABASE [current_database_name]  
MODIFY NAME = [new_database_name];  

  

### Select Queries

SELECT column[s]

FROM table

WHERE condition ( for some columns)

GROUP BY (arrange identical data into groups )

ORDER BY column_name (**sort the fetched data** in either ascending or descending according to one or more columns)

HAVING (filter based on condition)

  

|   |   |
|---|---|
|Having|Where|
|In the HAVING clause it will check the condition in group of a row.|In the WHERE condition it will check or execute at each row individual.|
|HAVING clause can only be used with aggregate function.|The WHERE Clause cannot be used with aggregate function like Having|
|Priority Wise HAVING Clause is executed after Group By.|Priority Wise WHERE is executed before  Group By.|

**Select Distinct:**  
The  
`SELECT DISTINCT` command returns only distinct (different) values in the result set. The clause considers NULL to be a unique value.

  

RANK:

> RANK() OVER (
> 
> [PARTITION BY expression, ]
> 
> ORDER BY expression (ASC | DESC) );

Rank will give gaps if same values. To not have gaps use dense_ranks;

### Join Queries

SQL JOIN clause is used to query and access data from multiple tables by establishing logical relationships between them

  

```SQL
SELECT * FROM table t1 JOIN table t2 ON [condition between t1 & t2 ]
```

  

1. Inner Join : The **[INNER JOIN](https://www.geeksforgeeks.org/sql-inner-join)** keyword selects all rows from both the tables as long as the condition is satisfied.

![[Images/Untitled 15.png|Untitled 15.png]]

```SQL
SELECT table1.column1,table1.column2,table2.column1,....
FROM table1
INNER JOINtable2
ON table1.matching_column = table2.matching_column;
```

1. Left Join: LEFT JOIN returns all the rows of the table on the left side of the join and matches rows for the table on the right side of the join. For the rows for which there is no matching row on the right side, the result-set will contain _null_.

![[attachments/Untitled 1 6.png|Untitled 1 6.png]]

1. Right Join: Similar to left except all rows of right will be there and only matching rows of left side
2. Full Join: **[FULL JOIN](https://www.geeksforgeeks.org/sql-full-join)** creates the result-set by combining results of both LEFT JOIN and RIGHT JOIN. The result-set will contain all the rows from both tables. For the rows for which there is no matching, the result-set will contain _NULL_ values.
3. Natural Join: join tables based on the common columns in the tables being joined.
4. Self Join: a self join is a regular join that is used to join a table with itself. It basically allows us to combine the rows from the same table based on some specific conditions.

```sql
SELECT columns
FROM table AS alias1
JOIN table AS alias2 ON alias1.column = alias2.column;
```

