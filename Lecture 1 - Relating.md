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

Study this in-depth using examples present on the harvard website and w3schools, here are the links : 

https://cs50.harvard.edu/sql/notes/1/#join

https://www.w3schools.com/sql/sql_join.asp

#Todo  Maybe add more stuff related to JOINS here, particularly interview related or helpful for interview.

### LEFT JOIN & RIGHT JOIN
Consider a situation where there are two tables namely `sea_lions` and `migrations` in a database. The first table has the data about individual sea lions : id and a name attached to them.
The migrations table has data about the migration of sea lions : id and distance (traveled by each sea lion).

Now in this situation, there might be a couple of scenarios where we have stopped tracking some of the sea lions and maybe in some cases we have a few sea lions on whom we did not even start our study of migration but still they were entered in out `sea_lions` table.

Now using **INNER JOIN** we can create a SQL query that will have common ids on both the tables and thus will return those records, but as specified earlier there might be some ids on the sea_lions table that aren't in the migrations table and there might be some migrations data on the migrations table which don't have a data on the sea_lions table!!
Thus there are certain gaps in these 2 tables, if we have to get the results according to either of them then we can use **LEFT JOIN** or **RIGHT JOIN** based on this.

Both LEFT JOIN and the RIGHT JOIN are the same thing, the only difference is the direction or basically the base table, with LEFT JOIN the base table is Left one or the first table mentioned in the query and with RIGHT JOIN, it is the right table or the table mentioned later on in the query.
So consider this : 

#Example  Consider this image showing both tables : 

![[Pasted image 20260603113546.png]]

Looking at the ids in both we can clearly see the gaps.
So, to know the migration done by the sea_lions in the sea_lions table we can just INNER JOIN as mentioned earlier, but to see the gaps we will use LEFT JOIN (where we put sea_lions table first)

``` SQL
SELECT *
FROM "sea_lions"
LEFT JOIN "migrations" ON "migrations"."id" = "sea_lions"."id";
```

This query would retain all sea lion data from the `sea_lions` table — the left one. Some rows in the joined table could be partially blank. This would happen if the right table didn’t have data for a particular ID. Here is the result from PostgreSQL:

![[Pasted image 20260603120238.png]]


If we look carefully, the data for Jolee (id 11790) is not present in the migrations table and thus we will see **Jolee** having blank data (or null) in the joined table.

#Question  Can we use JOINS on more than 2 tables?

Absolutely. In fact, joins are **not limited to two tables**. We can join as many tables as needed.
The pattern is simply:

``` SQL
SELECT ...
FROM table1
JOIN table2
    ON condition
JOIN table3
    ON condition
JOIN table4
    ON condition;
```


#Question  If we are trying to join three tables, how can we know which the left or right tables are?

For each JOIN statement, the first table before the keyword is the left one. The one that is involved in the JOIN keyword is the right table.

### FULL JOIN
A `FULL JOIN` allows us to see the entirety of all tables.
![[Pasted image 20260603121139.png]]

Notice the null values. Both the left and right tables are joined and the data missing in the combined form is stated as null.

#Todo  Study Natural Join, Not very important, in general avoided as it is risky!!


# SETS
On running a query in SQL, the results we see are called a **Result Set**.
This is a kind of set in SQL.

The idea comes from **set theory in mathematics**.
A table result can be thought of as a set of rows, and SQL provides operations to combine or compare those sets.

So using these SQL Set Operations, we can create queries which can help us find common data, merge result sets or 2 queries, find the complement or basically do those typical set ops that we did in Mathematics. 


### UNION
The `UNION` operator is used to combine the result set of two or more SELECT statements.

The `UNION` operator automatically **removes duplicate rows** from the result set.
Now, there are certain requirements for performing this `UNION` operation :

1) Every `SELECT` statement within `UNION` must have the same number of columns OR in other words, the 2 tables on which `UNION` is performed should have the same number of columns. 
2) The columns must also have similar data types.
3) The columns in every `SELECT` statement must also be in the same order.

#Syntax 

``` SQL
SELECT _column_name(s)_ FROM _table1_  
UNION  
SELECT _column_name(s)_ FROM _table2_;
```

#Example  Consider the same longlist.db database which had these 2 tables : `authors` and `translators` : 
If we wanted to know about those people who are either an author or a translator, or both, then they belong to the `UNION` of these 2 tables or basically result sets. In other words, we can get this particular *either-or* set by combining the author and translator sets.

![[Pasted image 20260604113637.png|395]]

``` SQL
SELECT "name" FROM "translators"
UNION
SELECT "name" FROM "authors";
```

Notice that every author and every translator is included in this result set, but only once!

A minor adjustment to the previous query gives us the profession of the person in the result set, based on whether they are an author or a translator.

``` SQL
SELECT 'author' AS "profession", "name" 
FROM "authors"
UNION
SELECT 'translator' AS "profession", "name" 
FROM "translators";
```

The above query will create a new column in the result set called "profession" which will have either 'author' OR 'translator' in them based on what table they come from.


### UNION ALL
Same as above. One simple change : 
The `UNION ALL` operator includes all rows from each statement, **including any duplicates**.

Same set of [[#UNION|requirements]] as above.


### INTERSECT
This operation is simply used to perform intersection. 
In other words, returns only rows present in both the sets.

#Example  

``` SQL
SELECT "name" from "Employees"
INTERSECT
SELECT "name" from "Customers";
```

Let's consider another example from the longlist.db series : 

#Example  In our database of books, we have authors and translators. A person could be either an author or a translator. If the two sets have an intersection, it is also a possible that a person could be both an author and a translator of books. We can use the `INTERSECT` operator to find this set.

![[Pasted image 20260604114252.png|410]]

``` SQL
SELECT "name" FROM "translators"
INTERSECT
SELECT "name" FROM "authors";
```


### EXCEPT
Returns rows from the first query that are not present in the second.

We can also think of this as 'ONLY X', like there are suppose tables of `Authors` and `Translators` and there is some aspect common in between them too. So you may want to know about the people who are 'ONLY AUTHORS'. In these kinds of situation, the `EXCEPT` operator is used.

![[Pasted image 20260604114938.png|390]]

``` SQL
SELECT "name" FROM "authors"
EXCEPT
SELECT "name" FROM "translators";
```

Similarly, it is possible to find a set of people who are only translators using `EXCEPT` (just flip the tables in the above query).

#Question  How can we find this set of people who are either authors or translators but not both?

![[Pasted image 20260604115215.png|365]]
The above Venn diagram is what we are looking for in the question!!

#Todo  The above question.


#Question  Could we use `INTERSECT`, `UNION` etc. to perform operations on 3-4 sets?  

Yes, absolutely. To intersect 3 sets, we would have to use the `INTERSECT` operator twice. An important note — we have to make sure to have the same number and same types of columns in the sets to be combined using `INTERSECT`, `UNION` etc.



# GROUPS
`GROUP BY` exists because sometimes you're not interested in individual rows anymore.

Instead, you want to treat multiple rows as a **single group** and perform calculations on that group.

The `GROUP BY` statement is used to group rows that have the same values into summary rows, like "Find the number of customers in each country".

The `GROUP BY` statement is almost always used in conjunction with aggregate functions, like `COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()`, to perform calculations on each group.

#Syntax  

``` SQL
SELECT _column1, aggregate_function(column2), column3, ..._  
FROM _table_name_  
WHERE _condition_  
GROUP BY _column1_, _column3_  
ORDER BY _column_name_;
```

#Example  Consider a situation where you have a table called **ratings** (from the same longlist.db database) and this is the structure of that : 

| book_id | rating |
| ------- | ------ |
| 1       | 4      |
| 1       | 3      |
| 1       | 4      |
| 2       | 2      |
| 2       | 3      |
So on this table if i run :

``` SQL
SELECT AVG(rating) FROM "ratings";
```

The above query would give me the average rating of all the books, but what if we want to see the average rating of each individual book, this is where we have to **GROUP** the ratings according to the `book_id` :

``` SQL
SELECT "book_id", AVG("rating") AS "average rating"
FROM "ratings"
GROUP BY "book_id";
```

In the above query, the `GROUP BY` keyword was used to create groups for each book and then collapse the ratings of the group into an average rating!

If we did NOT use the avg aggregate function, then it would just group the rows based on the column which in this case is `book_id` but from our example table, it seems they are already grouped together.

#Question  Now what if you want to apply some filtering logic on these groups?

Simple, we should use `WHERE`? 
No❌

This is the caveat here :)

In SQL, filtering logic keyword for individual rows and groups are different.
We used `WHERE` for individual rows, and we cannot use that on groups.
Think about it, what we just did in the above query!!
We are not displaying the individual rows, instead a result that is based on those individual rows, and thus we have to use a different keyword provided by SQL to apply filtering logic on `GROUPS` that is `HAVING` keyword, also termed as `HAVING CLAUSE`.

So, consider that we want to know about the books that have an average rating of >4 (out of 5)
Then,

``` SQL
SELECT "book_id", ROUND(AVG("rating"), 2) AS "average rating"
FROM "ratings"
GROUP BY "book_id"
HAVING "average rating" > 4.0;
```

#Question  What if, we wanted to count only those rows where the rating was >4 and then count the average of that, in this question, the filtering logic is not based on the groups, rather individual ratings!!
#Todo  above question

#Tip  Always question : What is the thing on which filtering logic is applied ?

If the answer is "individual rows" : `WHERE` Clause
If the answer is "groups" : `HAVING` Clause

#### In combination with ORDER BY
The basic logic to construct a SQL query becomes a very important aspect here. We need to know and have a sense of what comes first so that we don't always have to learn what to place where.

The general logic says, filter first order later. Think about it, we will filter records first and then only order them and at last limit them as well. So here this is an ideal #example of how to place ORDER BY along with GROUP BY and HAVING Clause :

``` SQL
SELECT "book_id", ROUND(AVG("rating"), 2) AS "average rating" 
FROM "ratings"
GROUP BY "book_id"
HAVING "average rating" > 4.0  --column we created above
ORDER BY "average rating" DESC
LIMIT 5;
```


----END OF LECTURE----

----
---





