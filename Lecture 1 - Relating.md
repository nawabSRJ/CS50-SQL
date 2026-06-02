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

``` SQL
CREATE TABLE Persons (  
    ID int PRIMARY KEY,  
    LastName varchar(255) NOT NULL,  
    FirstName varchar(255),  
    Age int  
);
```


#Example of creating a primary key on multiple columns : 

``` SQL
CREATE TABLE Persons (  
    ID int,  
    LastName varchar(255),  
    FirstName varchar(255),  
    Age int,  
    PRIMARY KEY (ID, LastName)  
);
```


Notice : How during multiple columns the format completely changed, in this we do NOT omit defining the columns earlier, but just push them later like arguments of PRIMARY KEY constraints.

#Todo  Named vs Un-named primary key

#Todo  Learn how to use DROP and ALTER with PRIMARY KEY  to remove or add constraint.

### Foreign Key
As we learned earlier, keys also help establish relations between tables. Foreign is exactly what helps us in this task.

A foreign key is a primary key taken from a different table. By referencing the primary key of a different table, it helps relate the tables by forming a link between them. Also, the foreign key ensures that it prevents the actions that may destroy the link between the tables thus maintaining relational integrity.

So, in very simple terms : A FOREIGN KEY is a column in a table that refers to the PRIMARY KEY in another table.

The table with the foreign key column is called the `child table`, and the table with the primary key column is called the `referenced or parent table`.

#Important  The `FOREIGN KEY` constraint also prevents you from deleting a record in the parent table, if related rows still exist in the child table. 

![[Pasted image 20260602140235.png]]

Notice how the primary key of the `books` table is now a column in the `ratings` table. This helps form a **one-to-many relationship** between the two tables — a book with a title (found in the `books` table) can have multiple ratings (found in the `ratings` table).

#Tip  We don't necessarily need to use some ISBN kind of value here or even in other scenarios, the very requirement of a unique attribute attached to a particular entity may not be present and thus we can construct our own unique primary keys as well like simple numbers : 1,2,3,4.... and so on as long as each book (or entity) has a unique number to identify it.

#Example of creating FOREIGN KEY constraint while defining a table:

``` SQL
CREATE TABLE Orders(
	OrderID INT PRIMARY KEY,
	OrderNumber INT NOT NULL,
	PersonID INT,
	CONSTRAINT fk_Person
	FOREIGN KEY (PersonID)
	REFERENCES Persons(PersonID)
);
```

In the above syntax, SQL creates a FOREIGN KEY constraint on the `PersonID` column upon creation of the `Orders` table.

`CONSTRAINT fk_Person` is **just giving a name to the foreign key constraint**. It is not what creates the foreign key. The actual foreign key is created by:
``` SQL
FOREIGN KEY (PersonID)
REFERENCES Persons(PersonID)
```

We can also define the FOREGIN KEY without a constraint name, the SQL will do assign a name automatically : 

``` SQL
CREATE TABLE Orders(
    OrderID INT PRIMARY KEY,
    OrderNumber INT NOT NULL,
    PersonID INT,
    FOREIGN KEY (PersonID)
    REFERENCES Persons(PersonID)
);
```

This above syntax works perfectly fine!!

#Todo  Study more on constraint names and how do they help manage different constraints - dropping and altering constraints.


#Question  Can we create a FOREIGN KEY constraint on an already created table or after creating a table?

Answer : Yes, here is how we can ALTER a table to add FOREIGN KEY constraint:

``` SQL
ALTER TABLE Orders  
ADD CONSTRAINT fk_Person  
FOREIGN KEY (PersonID)  
REFERENCES Persons(PersonID);
```


#Todo  Study other keys which are just theoretical concepts, you don't define them but maybe asked in interviews : Candidate key, Alternate key, Super key, Composite key.


# SUBQUERIES
A subquery is a query inside another query. These are also called **nested queries**.

#Example  Consider this example for a one-to-many relationship. In the `books` table, we have an ID to indicate the publisher, which is a foreign key taken from the `publishers` table. To find out the books published by **Fitzcarraldo Editions**, we would need two queries — one to find out the `publisher_id` of Fitzcarraldo Editions from the `publishers` table and the second, to use this `publisher_id` to find all the books published by Fitzcarraldo Editions. These two queries can be combined into one using the idea of a subquery.

``` SQL
SELECT "title"
FROM "books"
WHERE "publisher_id" = (
    SELECT "id"
    FROM "publishers"
    WHERE "publisher" = 'Fitzcarraldo Editions'
);
```

* The subquery is solved first then the outer query.
* The query that is furthest inside or the inner most query inside parentheses will be run first, followed by outer queries chronologically.

#Tip  The inner query is indented. This is done as per style conventions for subqueries, to increase readability.

#Question  To find all the ratings for the book In Memory of Memory

``` SQL
SELECT "rating"
FROM "ratings"
WHERE "book_id" = (
    SELECT "id"
    FROM "books"
    WHERE "title" = 'In Memory of Memory'
);
```

#Question  To select just the average rating for this book

``` SQL
SELECT AVG("rating")
FROM "ratings"
WHERE "book_id" = (
    SELECT "id"
    FROM "books"
    WHERE "title" = 'In Memory of Memory'
);
```

#Example  #Question  The next example is for many-to-many relationships. To find the author(s) who wrote the book Flights, three tables would need to be queried: `books`, `authors` and `authored`.

``` SQL
SELECT "name"
FROM "authors"
WHERE "id" = (
    SELECT "author_id"
    FROM "authored"
    WHERE "book_id" = (
      SELECT "id"
      FROM "books"
      WHERE "title" = 'Flights'
    )
);
```

* In the above syntax, first query that is run is the most deeply nested one — finding the ID of the book Flights. Then, the ID of the author(s) who wrote Flights is found. Last, this is used to retrieve the author name(s).


# IN 
This keyword is used to check whether the desired value is _in_ a given list or set of values.

#Example  The relationship between authors and books is many-to-many. This means that it is possible a given author has written more than one book. To find the names of all books in the database written by Fernanda Melchor, we would use the `IN` keyword as follows :

``` SQL
SELECT "title"
FROM "books"
WHERE "id" IN (
    SELECT "book_id"
    FROM "authored"
    WHERE "author_id" = (
        SELECT "id"
        FROM "authors"
        WHERE "name" = 'Fernanda Melchor'
    )
);
```

In the above query : 
`Authors` Table has details about Authors.

`Authored` Table has mapping of author id with book id ~ that why's it will give a range of IDs and it is inside of the first query.

`Books` Table has the details of Books.

#Question  What if the value of an inner query is NOT found?

In that case, the query (not just inner but any query for that matter) returns nothing, thereby prompting the outer query to also return nothing. 

#Important  The outer query is thus dependent on the results of the inner query.


# JOINS
This keyword allows us to combine two or more tables together.
The `JOIN` clause is used to combine rows from two or more tables, based on a related column between them.

### Types of Joins 
- `(INNER) JOIN`: Returns only rows that have **matching values** in both tables.

- `LEFT (OUTER) JOIN`: Returns **all rows from the left table**, and only the matched rows from the right table.

- `RIGHT (OUTER) JOIN`: Returns **all rows from the right table**, and only the matched rows from the left table.

- `FULL (OUTER) JOIN`: Returns **all rows** when there is a match in either the left or right table