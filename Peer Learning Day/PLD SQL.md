# SQL
## What is a database, and what is its main purpose?

database is a way to store data in an orginized way it aims at having datas that make sense together and deal with relationship if needed
## What is a relational database, and how does it differ from a non-relational one?

a relational database is a database that uses relations to store data typically you have multiple data tables that can be connected with eachother using an id.
a non relational one just stores data without dealing with multiple tables it's one big table

## What does SQL stand for, and what is it used for?

Structured Query Language it is used for making sql request in a database that uses the sql language like mariadb (mysql), postgresql or sqlite

## What is MySQL, and how does it differ from other SQL database engines?

mySQL is database server it differs from other sql database because it lacks the complexity of postgree which makes it lighter but it also does store datas the traditional way in multiple files unlike SQLite

## How do you connect to a MySQL server from the terminal?

```bash
sudo mysql -uroot -p
```

## How do you create a new database in MySQL?
```sql
CREATE TABLE IF NOT EXISTS new_table;
```

## What are DDL and DML statements, and what is the difference between them?

Data Definition Language and Data Manipulation Language, 
CREATE, ALTER, DROP (DDL), SELECT, INSERT, UPDATE, DELETE (DML)

## How do you create a new table in MySQL?

```sql
CREATE TABLE IF NOT EXISTS new_table(
	...
);
```

## How can you modify an existing table using the ALTER TABLE command?

```sql
ALTER TABLE nom_table
instruction;
```
## How do you insert new data into a table using SQL?
```sql
INSERT INTO nom_table
VALUES ()
;
```

## How do you update or delete specific records in a table?
UPDATE
```sql
UPDATE table
SET nom_colonne_1 = 'nouvelle valeur'
WHERE _**condition**_
```
## What is a PRIMARY KEY, and why is it important?

a primary key is for identifying a table, it is important to avoid duplicate datas and for relationships

## What is a FOREIGN KEY, and how does it help maintain data integrity?

a Foreign key references the PRIMARY KEY of another table it help maintain data integrity by not having to have a massive table with multiple rows

## What is the purpose of the NOT NULL and UNIQUE constraints?

NOT NULL does not allow for inserting a null value in a table
UNIQUE doesn't allow for duplicate value in a table

## What is the difference between CHAR, VARCHAR, and TEXT data types in MySQL?

CHAR is a fixed sized string
VARCHAR is a VARIABLE sized string
TEXT Holds a string with a maximum size of 65,535 bytes

## How can you retrieve data from multiple tables in a single query?

```sql
SELECT table1.data table2.data
FROM table1
INNER JOIN table2
ON table1.table_id = table2.id;
```
you use JOIN

## What is the difference between an INNER JOIN, LEFT JOIN, and RIGHT JOIN?

INNER JOIN takes datas that are in both tables only
LEFT JOIN takes all datas from LEFT and the datas that are connected to right only
RIGHT JOIN takes all datas from RIGHT and the datas that are connected to left only

## What is the difference between a JOIN and a UNION in SQL?

A Join combines datas from muiple tables while a union merge them in the same table you need to have similar data types and row amount in order to do a UNION

## What are subqueries, and when should you use them?

subquerries are queries inside queries, you should use them when you need to filter specific datas or when doing an aggregation

## What are SQL functions, and can you give examples of common ones (e.g., COUNT, AVG, MAX, MIN)?

SQL functions are aggregations they can be used to do some mathematical operation
example:
COUNT, AVG, MAX, MIN, SUM, VAR

## How can you group and filter results using GROUP BY and HAVING clauses?

GROUP BY
```sql
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

## How do you order query results using ORDER BY, and what is the default sort order?

ORDER BY id (ASC DESC)

## How do you create a new MySQL user, and how do you grant privileges to that user?

```sql
CREATE USER 'new_user'@'localhost' INDENTIFIED BY 'some random password';
GRANT ALL privilledge ON *.* TO new_user;
```

## How can you list all users and their privileges in a MySQL server?

SHOW GRANTS;

## What are the benefits of database normalization, and what problems does it help to avoid?

the beenfits of database normalization is a reduction in data redunduncy and better data integrity
it's to avoid data anomaly and better querries

# Python - Object-relational mapping

## How can you connect to a MySQL database using the MySQLdb module in Python?

## What is the difference between executing a query with MySQLdb and using SQLAlchemy?

## What is the role of a cursor in the MySQLdb module, and how do you retrieve query results from it?

## What is an SQL injection, and how can you prevent it in Python scripts that use MySQLdb?

## What is an Object-Relational Mapper (ORM), and why is it useful in software development?

## How do you map a Python class to a MySQL table using SQLAlchemy?

## What is the purpose of the Base = declarative_base() object in SQLAlchemy?

## How do you create, read, update, and delete (CRUD) objects using an SQLAlchemy session?

## How do you define a one-to-many relationship (for example, between State and City) in SQLAlchemy?

## What are the main advantages and possible drawbacks of using an ORM compared to writing raw SQL queries?
