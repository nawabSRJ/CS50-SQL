Databases can have multiple tables. In last lecture we talked about a CINEMA Table, this time let's refer to the same book or longlist table as in the CS50 lectures.

In general the command to see the tables in a database is : 

`SHOW TABLES`

But when using **SQLite**, we will use : 

`.tables` 

This command will return the names of the tables present in the database that we are working on.
In SQLite to select a database simply :

#Syntax   `sqlite3 dbname.db`

#Example  `sqlite3 longlist.db`

Note that in **sqlite3** , the '3' is the version of sqlite.

# RELATION AMONG TABLES
Now obviously the tables in a particular database would have some kind of correlation, as we always follow the policy of grouping a similar kind of data. So in this longlist.db database consisting of the books nominate for international prize for over a period of few years, we would have the following info : book title, author, translator, rating, votes etc.

Now, these tables as we know would have some relationship between them, and hence we call the database a **RELATIONAL DATABASE**.

Now, we have to think, about the relationship these tables can have among them and also the **kind** of relationship, consider this : 
- Authors write books.
- Publishers publish books.
- Books are translated by translators.

Read https://cs50.harvard.edu/sql/notes/1/ to explore in depth about the examples or methods that can be used to arrange these tables. 
We will here focus on what matters.

All in all, we need to think of what is an entity, if something is an entity then we have to most probably create a table for that and then we need to build relationship among those entities. 
#Example A table for BOOKS as it is an entity, and a table for AUTHORS of those books as they are an entity too. And then we use SQL based constraints and keys etc. to establish the relationship between these 2 entities.

# TYPE OF RELATIONSHIPS
In SQL, the type of or the kind of relationship between tables can be of : 

1) One to One
2) One to Many
3) Many to Many
4) Many to One

  
Consider this case, where each author writes only one book and each book is written by one author. This is called a **one-to-one relationship** :

![[Pasted image 20260531135428.png|470]]

  
On the other hand, if an author can write multiple books, the relationship is a **one-to-many relationship** :

![[Pasted image 20260531135549.png|444]]

  
Here, we see another situation where not only can one author write multiple books, but books can also be co-written by multiple authors. This is a **many-to-many relationship** :

![[Pasted image 20260531135705.png|520]]

Also, **Many to One Relationship** can be considered in a scenario where we talk about students and teachers, say many or multiple students in a group project have a single mentor (or teacher), thus establishing Many to One from student's perspective.



# ER DIAGRAM
Now that we know in a database tables can have relations with each other, we need to figure out how to access or how to show what table is related to which one? For this we have ER Diagram.

An ER Diagram is a pictorial way to lay out the relationships among different entities (or tables) using a predefined set of symbols and rules, which helps in designing and visualizing the database structure efficiently.

Well, now besides the visualization part of the ER Diagram, we need to know the **WHY?**

#Question  Why do we need an ER Diagram in the first place? 

We all know the WHAT of ER Diagram, that it helps in visualization, but why do we even need visualization and thus why even an ER Diagram?

The reason is : ER diagrams help you design the database correctly before writing a single SQL query.

Think of this like : You wouldn't start constructing a building by randomly placing walls and rooms. You first create a blueprint to answer questions like:

- How many rooms are needed?
- Which rooms connect to which?
- Where are the doors?
- What is the purpose of each room?

**ER diagrams do exactly that for databases**

#Question  If we have some database, how do we know the relationships among the entities stored inside of it?

The exact relationships between entities are really up to the designer of the database. For example, whether each author can write only one book or multiple books is a decision to be made while designing the database. An ER diagram can be thought of as a tool to communicate these decisions to someone who wants to understand the database and the relationships between its entities.

#Todo  More on this ER Diagram in a different file or maybe read on the CS50 link present in the  [[#RELATION AMONG TABLES]] section.


# KEYS
Relations are the foundation of the RDBMS systems and paradigm. We studied in the [[#ER DIAGRAM]] section about how to depict these relations, but now we also have to know how do we implement these relationships among tables.

This is where the concept of **KEYS** comes in SQL. 

Keys are database constraints consisting of a single column or a group of columns used to uniquely identify rows, establish relationships between tables, and enforce data integrity. 

Think of a key as an **identity mechanism**.
Consider a table:

| student_id | name  | email                                     |
| ---------- | ----- | ----------------------------------------- |
| 101        | John  | [john@gmail.com](mailto:john@gmail.com)   |
| 102        | Alice | [alice@gmail.com](mailto:alice@gmail.com) |
How do we uniquely identify John?
Not by name (there could be multiple Johns).
Instead: student_id = 101
This makes `student_id` a key candidate.

#### Why Do We Need Keys?

Without keys:
- Duplicate records become difficult to detect.
- Relationships between tables cannot be established.
- Data integrity cannot be maintained.
- Finding specific records becomes unreliable.

**Keys solve these problems**

### Primary Key
An identifier that is unique for every item in the table.
The `PRIMARY KEY` constraint uniquely identifies each record in a database table.

A `PRIMARY KEY` constraint ensures unique values, and cannot contain NULL values and thus it is a combination of both a `UNIQUE` constraint and a `NOT NULL`

A table can have only ONE `PRIMARY KEY` constraint. The primary key can either be a single column, or a combination of columns.

#Example of creating a primary key in a single column : 

CREATE TABLE Persons (  
    ID int PRIMARY KEY,  
    LastName varchar(255) NOT NULL,  
    FirstName varchar(255),  
    Age int  
);

#Example of creating a primary key on multiple columns : 

CREATE TABLE Persons (  
    ID int,  
    LastName varchar(255),  
    FirstName varchar(255),  
    Age int,  
    PRIMARY KEY (ID, LastName)  
);

Notice : How during multiple columns the format completely changed, in this we do NOT omit defining the columns earlier, but just push them later like arguments of PRIMARY KEY constraints.

#Todo  Named vs Un-named primary key

#Todo  Learn how to use DROP and ALTER with PRIMARY KEY  to remove or add constraint.

### Foreign Key
