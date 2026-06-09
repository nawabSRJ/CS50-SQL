In this lecture we will learn how to design our own database schemas efficiently.

Note that some commands discussed here are not SQL keywords, they are just **sqlite commands** applicable to that particular database.

This command is used to know how the database was created, it will show you all the CREATE TABLE commands for all the tables.
``` SQL
.schema;
```

In case you wanted to see schema of a particular table : 

``` SQL
.schema tablename;
```

On running this, we see the SQL statement used to create the table `tablename`.
This shows us the columns inside `tablename` and the types of data that each column is able to store.

In general if you want to select a particular database we use : 

``` SQL
Use DbName;
```

And if we have to take a look at the tables present in this database then we type:

``` SQL
Show Tables;
```


# CREATING OUR OWN SCHEMA
For this assignment : we are tasked with representing the subway system of the city of Boston through a database schema. This includes the subway stations, the different train lines, and the people who take the trains (Acc. to Lecture - 2 of CS50 SQL).

![[Pasted image 20260605122036.png|468]]

To break down the question further, we need to decide:
1) what kinds of tables we will have in our Boston Subway database,
2) what columns each of the tables will have, and
3) what types of data we should put in each of those columns.

These are the types of questions we have to ask before designing any database.
This is where we understand the concept of **Normalization**.


# NORMALIZATION
To **normalize** means to reduce redundancies, effectively, to take one big table with multiple distinct columns and entities and split that up in different tables and let each entity be part of it's very own table.

So the ultimate goal becomes to :
1) Reduce unnecessary duplication of data
2) Improve data consistency
3) Prevent data anomalies
4) Make relationships between data explicit

The key highlight here is identifying "**Entities**" and then putting them in their own table. Make sure even if you were to add a column beyond basic needs, the new column should deal with data about that same entity.

#Example  Consider this table : 

Riders

| id  | name    |
| --- | ------- |
| 1   | Charlie |
| 2   | Alice   |
| 3   | Bob     |
Now even if i had to add a column in the above table, that particular column data should be related to Riders only and not any other thing, say stations, fare, etc.

This process helps us to make database that is more dynamic, more easy to reproduce, and more easy to query or write queries on.

## Normalization Types
Each normal form asks:

> "Is there still some bad design pattern remaining in this table?"

If yes, we move to the next normal form.

``` 
Raw Table
   ↓
1NF → Remove messy values
   ↓
2NF → Remove partial dependencies
   ↓
3NF → Remove indirect dependencies
   ↓
BCNF → Handle remaining candidate key anomalies
```

### 1NF
**Purpose** : Ensure that each cell contains a single atomic value.
**Rule** : A column should contain **One Value**. Not a list, not multiple values, not repeating groups.

**Bad Design** :

| student_id | name | courses     |
| ---------- | ---- | ----------- |
| 1          | John | SQL, Python |

**1NF Version** :

|student_id|name|course|
|---|---|---|
|1|John|SQL|
|1|John|Python|
1NF converts data into a structure that SQL can efficiently query.
Without 1NF, relational databases lose many of their advantages.

### 2NF
**Purpose** : Remove dependencies on only part of a composite key.
This only matters when: **Primary Key = multiple columns**
**Rule** : Every non-key attribute must depend on the entire primary key. Not just part of it.

#Example  Suppose :

|student_id|course_id|student_name|
|---|---|---|
Now here the primary key : (student_id, course_id)
#Question : Does student_name depend on (student_id, course_id) 
No.
It depends only on "student_id".
This is called **Partial Dependency** because the column depends on only part of the key.

**2NF Fix :**
Split into : 
### Students

| student_id | student_name |

### Enrollments

| student_id | course_id |

2NF ensures information is stored with the entity it actually describes.

**Understanding Partial Dependencies**
A partial dependency occurs when a non-key column depends on only part of a composite primary key, rather than the whole key. To achieve 2NF, every non-key column must depend on the entire primary key.
If a partial dependency exists, you must break the table into separate tables to separate the data and ensure every column relies only on the primary key.

**Example of a 2NF Violation** 
Imagine an `Orders` table where the composite primary key is a combination of `{OrderID, ProductID}`:

- **OrderID, ProductID** (Primary Key)
- **Quantity**
- **ProductPrice** 

Here, `ProductPrice` depends only on the `ProductID`, not the entire primary key. Because `ProductPrice` only depends on a subset of the primary key, it is a **partial dependency** and the table is **not in 2NF**. 

#todo  3NF and BCNF


# CREATE TABLE
Used to create a new table in the database. Before using this command we need to make sure that we are in the correct database, different software have a little bit of difference in creating and using databases.

General way of creating database/ MySQL/ SQL SERVER/ PostgreSQL:

``` SQL
CREATE DATABASE Dbname;
```

Connecting to a database : Remember that in most SQL software including the PostgreSQL, you don't really exit a database, you just switch to another, using : 

``` SQL
\c Dbname;
```

'Dbname' is the name of the database you want to switch to.
PostgreSQL automatically disconnects from the current database and connects to the new one.

In other database such as MySQL/ SQL SERVER it is:

``` SQL
USE Dbname;
```

In Sqlite, the command to create and to use a database are the same, it's like the practical use of POLYMORPHISM, now here the database being created or used depends on it's presence, suppose a database with name 'Library' is already created then when you type : 

``` Sqlite
Sqlite3 Library;
```

Then the 'Library' database will be selected otherwise if it does NOT exist, then it will be created with the help of the same above command;

Note that : In Sqlite you need to **quit** a database to join another, using the command : 

``` Sqlite
.quit
```
or 
``` Sqlite
.exit
```

Now coming back to the original topic : **CREATE TABLE**

#Syntax 

``` SQL
CREATE TABLE _table_name_ (  
  _column1 datatype constraint_,  
  _column2 datatype constraint_,  
  _column3 datatype constraint_,  
  ....  
);
```

#Example 

``` SQL
CREATE TABLE Persons (  
  PersonID int PRIMARY KEY,  
  LastName varchar(255) NOT NULL,  
  FirstName varchar(255),  
  Address varchar(255),  
  City varchar(255)  
);
```

#Tip  We can also create a new table from an existing table. Here is how to do it :

#Syntax 

``` SQL
CREATE TABLE _new_table_ AS  
SELECT _column1, column2,..._  
FROM _existing_table_  
WHERE ....;
```

#Example 

``` SQL
CREATE TABLE GermanCustomers AS  
SELECT * FROM Customers  
WHERE Country = 'Germany';
```


# DATATYPES & STORAGE CLASSES
This is a very varied topic among different SQL Systems. As a good developer, it is important to know all of them, so we will create new files for each of them and hence explore different syntaxes for different groups. Similar syntaxes will be grouped usually.
Please refer : [[Sqlite Datatypes]] and [[Standard SQL Datatypes]]



# DROP TABLE
The `DROP TABLE` statement is used to permanently delete an existing table in a database.
Be careful before dropping a table! Dropping a table deletes the entire table and all its content!

#Syntax 

``` SQL
DROP TABLE table_name;
```


To prevent an error from occur (if the table does not exists), it is a good practice to add the `IF EXISTS` clause:

``` SQL
DROP TABLE IF EXISTS table_name;
```

**Very Important Note** : In most databases you cannot drop a table that is referenced by a FOREIGN KEY in another table. To solve this, you must remove the foreign key constraint or drop the dependent table. 
*OR*
In other words, you **cannot drop** the **Parent Table** (the one containing the PRIMARY KEY which is REFERENCED in another table).


# TRUNCATE TABLE
The `TRUNCATE TABLE` statement is used to delete all the records in a table, but it keeps the table structure, columns and constraints.

#Syntax 

``` SQL
TRUNCATE TABLE _table_name_;
```



# TABLE CONSTRAINTS
We can use table constraints to impose restrictions on certain values in our tables.
A constraint is a **rule enforced by the database** that restricts what data can be stored.
Constraints provide **Data Integrity**, without them keeping a check on what kind of data is being added becomes tough to manage.

**Note** : PRIMARY KEY, FOREIGN KEY are also considered as **Constraints**. 

### NOT NULL 
Value cannot be Null.

#Syntax 

``` SQL
name VARCHAR(50) NOT NULL
```

This column will NOT hold any NULL value. 

### UNIQUE
No duplicate values.

#Syntax 

``` SQL
email VARCHAR(100) UNIQUE
```

This column will NOT hold any 2 similar values OR no two rows can have same email attribute.

### CHECK
Expression must evaluate to TRUE.
OR
The value being passed should follow the rule.

#Syntax 

``` SQL
Age int CHECK (Age >= 18)
```

Adding check constraint using ALTER TABLE:

``` SQL
ALTER TABLE Persons  
ADD CHECK (Age >= 18);
```

Composite CHECK Constraint:
``` SQL
CREATE TABLE Employees (
    start_date DATE,
    end_date DATE,

    CHECK (end_date > start_date)
);
```


### DEFAULT
Use a default value when none is provided.

#Syntax 

``` SQL
country VARCHAR(50) DEFAULT 'India'
```

Very useful in scenarios where you need to add some fallback value OR you know what could possibly be the value if NOT provided explicitly.

``` SQL
CREATE TABLE Orders (  
    ID int PRIMARY KEY,  
    OrderNumber int NOT NULL,  
    OrderDate date DEFAULT CAST(GETDATE() AS date)  
);
```

Note : Sometimes we need to assign **default dates** to a certain column, for this there are in-built functions in different DBMS:

**MySQL**
``` SQL
CREATE TABLE Orders (  
    ID int PRIMARY KEY,  
    OrderNumber int NOT NULL,  
    OrderDate date DEFAULT CURRENT_DATE()  
);
```

**SQL SERVER**
``` SQL
CREATE TABLE Orders (  
    ID int PRIMARY KEY,  
    OrderNumber int NOT NULL,  
    OrderDate date DEFAULT CAST(GETDATE() AS date)  
);
```


### AUTO INCREMENT
Increment the value of a particular column automatically by a specified number.
This once again, has different keywords for different DBMS. The method overall is the same, defining the constraint while defining the column.

#Syntax  **For MySQL** :

MySQL uses the `AUTO_INCREMENT` keyword to perform an auto-increment feature.

The following SQL defines the `Personid` column to be an auto-increment primary key field in the "Persons" table:
``` SQL
CREATE TABLE Persons (  
    Personid int AUTO_INCREMENT PRIMARY KEY,  
    LastName varchar(255) NOT NULL,  
    FirstName varchar(255),  
    Age int  
);
```
The default starting value for `AUTO_INCREMENT` is 1, and it will increment by 1 for each new record.

To let `AUTO_INCREMENT` start with another value, use the following SQL statement:
``` SQL
ALTER TABLE Persons AUTO_INCREMENT = 100;
```

When we insert a new record into the "Persons" table, we will NOT have to specify a value for the `Personid` column, a unique value will be added automatically, and thus insertion query will be : 
``` SQL
INSERT INTO Persons (FirstName, LastName)  
VALUES ('Lars', 'Monsen');
```

#Syntax  **For SQL SERVER** :
The SQL Server uses the `IDENTITY` keyword to perform an auto-increment feature.

The following SQL defines the `Personid` column to be an auto-increment primary key field in the "Persons" table:
``` SQL
CREATE TABLE Persons (  
    Personid int IDENTITY(1,1) PRIMARY KEY,  -- IDENTITY(START, STEP)
    LastName varchar(255) NOT NULL,  
    FirstName varchar(255),  
    Age int  
);
```
In the example above, the starting value for `IDENTITY` is 1, and it will increment by 1 for each new record.

#Tip  To specify that the `Personid` column should start at value 10 and increment by 5, change it to `IDENTITY(10,5)`.

#Todo  Look for the PostgreSQL auto increment, it uses some ALWAYS, BY DEFAULT



### COLUMN LEVEL VS TABLE LEVEL CONSTRAINTS
Column level : Defined immediately after a column definition.
Table level : Declared separately after the column definitions.

#Question  Why Do Table-Level Constraints Exist?

Because some constraints involve multiple columns.


#Example  

Column Level

#Todo this seems out of my head right now



# ALTER TABLE
The `ALTER TABLE` statement is used to add, delete, or modify columns in an existing table.

The `ALTER TABLE` statement is also used to add and drop various constraints on an existing table.

Common `ALTER TABLE` operations are:

- Add column - Adds a new column to a table
- Drop column - Deletes a column in a table
- Rename column - Renames a column
- Modify column - Changes the data type, size, or constraints of a column
- Add constraint - Adds a new constraint
- Rename table - Renames a table


**ADD COLUMN**
To add a column in a table, use the following syntax:

#Syntax 

``` SQL
ALTER TABLE table_name  
ADD column_name datatype;
```

#Example  The following SQL adds an "Email" column to the "Customers" table:

``` SQL
ALTER TABLE Customers  
ADD Email varchar(255);
```

Again, a little different for PostgreSQL:

#Syntax 

``` PostgreSQL
ALTER TABLE table_name
ADD COLUMN column_name datatype;
```
Notice here that only one extra keyword 'COLUMN' is added after 'ADD' keyword

#Example 

``` PostgreSQL
ALTER TABLE employees
ADD COLUMN email VARCHAR(100);
```


**DROP COLUMN**
To delete a column in a table, use the following syntax (notice that some database systems don't allow deleting a column) :

#Syntax  *Valid for PostgreSQL*

``` SQL
ALTER TABLE table_name  
DROP COLUMN column_name;
```

#Example 

``` SQL
ALTER TABLE Customers  
DROP COLUMN Email;
```


**RENAME COLUMN**
To rename a column in a table, use the following syntax:

#Syntax  *Valid for PostgreSQL*

``` SQL
ALTER TABLE table_name  
RENAME COLUMN old_name to new_name;
```

#Example 

``` SQL
ALTER TABLE Customers
RENAME COLUMN Customers to IndiaCustomers
```

**Note** : The Syntax for this same thing in **SQL SERVER** is different. Here it is : 

``` SQL SERVER
EXEC sp_rename '_table_name.old_name_', '_new_name_', 'COLUMN';
```


**MODIFY DATATYPE**
To modify the data type, size or constraints of a column in a table, use the following syntax:

For MySQL/ Oracle:

``` SQL
ALTER TABLE table_name  
MODIFY column_name new_datatype constraint;
```


For SQL SERVER/ MS ACCESS:

``` SQL
ALTER TABLE _table_name_  
ALTER COLUMN _column_name new_datatype constraint_;
```


For PostgreSQL:

``` PostgreSQL
ALTER TABLE table_name
ALTER COLUMN column_name TYPE new_datatype;
```

#Example 

``` PostgreSQL
ALTER TABLE employees
ALTER COLUMN salary TYPE BIGINT;
```

Full **PostgreSQL** ALTER TABLE COMMANDS:
``` PostgreSQL
-- Add column
ALTER TABLE employees
ADD COLUMN email VARCHAR(100);

-- Drop column
ALTER TABLE employees
DROP COLUMN email;

-- Rename column
ALTER TABLE employees
RENAME COLUMN email TO work_email;

-- Rename table
ALTER TABLE employees
RENAME TO staff;

-- Change datatype
ALTER TABLE employees
ALTER COLUMN salary TYPE BIGINT;

-- Set NOT NULL
ALTER TABLE employees
ALTER COLUMN salary SET NOT NULL;

-- Remove NOT NULL
ALTER TABLE employees
ALTER COLUMN salary DROP NOT NULL;

-- Shortcut
ADD COLUMN
DROP COLUMN
RENAME COLUMN
RENAME TO -- for changing table name
ALTER COLUMN
```


#Tip  You will remember them when you practice a lot.












