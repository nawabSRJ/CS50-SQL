
# INSERT INTO
After creating tables, we have to put data inside it finally. The `INSERT INTO` command is used to insert data into a table or insert new records in a table. We can insert :
1) data of one row or multiple rows at the same time.
2) data of some columns not all (though the success of this depends on the column definition)

#Syntax  Specify both the column names and the values to be inserted:

``` SQL
INSERT INTO table_name (column1, column2, column3, ...)  
VALUES (value1, value2, value3, ...);
```

#Syntax  If you insert values for ALL the columns of the table, you can omit the column names. However, the order of the values must be in the same order as the columns in the table:

``` SQL
INSERT INTO table_name  
VALUES (value1, value2, value3, ...);
```

#Example  

``` SQL
INSERT INTO Customers (CustomerName, City, Country)  
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

**Note** : The order of column names follows the same order used while creating the table.
**Tip** : For reference purposes, look at the table schema first if confused about it. 

OR

``` SQL
INSERT INTO Customers
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
```

**Insert Multiple Rows** :

``` SQL
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)  
VALUES  
('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway'),  
('Greasy Burger', 'Per Olsen', 'Gateveien 15', 'Sandnes', '4306', 'Norway'),  
('Tasty Tee', 'Finn Egan', 'Streetroad 19B', 'Liverpool', 'L1 0AA', 'UK');
```

**Note** : Make sure you separate each set of values with a comma

#Important  When you are inserting values, make sure the constraints that were applied on the table while it was defined are followed otherwise you will run into a **RUNTIME ERROR**. 
Suppose you tried to insert the same email id in two different rows then the query will fail and no data will be entered.
Similarly, we can try to add a row with a `NULL` title, violating the `NOT NULL` constraint.

#Todo  There are methods using which we can insert csv data in a table rather than manually entering values one by one. Refer those methods.

#Important  The **INSERT** command does NOT work with **WHERE** clause, but the **DELETE** works, this means we can apply filters during deletion of records but NOT during the insertion.


# DELETE
The `DELETE` statement is used to delete existing records in a table.

#Syntax 

``` SQL
DELETE FROM table_name WHERE condition;
```

**NOTE** : Be careful when deleting records in a table! Notice the `WHERE` clause in the `DELETE` statement. The `WHERE` clause specifies which record(s) should be deleted. If you omit the `WHERE` clause, all records in the table will be deleted!

#Syntax  To delete all records :

``` SQL
DELETE FROM table_name;
```

#Example 

``` SQL
DELETE FROM "collections";
```

This above statement will all the rows of the "collections" table.

#Example 

``` SQL
DELETE FROM Customers WHERE CustomerName='Alfreds Futterkiste';
```


#Tip  Sometimes deletions means not just deleting the entries but the entire table itself, in these cases we have to `DROP` the table :

#Syntax  To delete the entire table and not just it's records : 

``` SQL
DROP TABLE table_name;
```

#Example 

``` SQL
DROP TABLE Customers
```

#Tip  If at times you are confused about the result set that could be eliminated using the `DELETE` statement that you have formed, it is advised that you first run the `SELECT` query on that statement.

Suppose you have to delete orders after or before a certain date, say before a certain date in our next example and you have formed this query : 

``` SQL
DELETE FROM Orders WHERE orderDate < '1-6-2026';  -- 1st June 2026
```

And after formulating this you are not sure about the records that will get deleted, so to be extra sure you just have to run this query : 

``` SQL
SELECT * FROM Orders WHERE orderDate < '1-6-2026';
```

#Tip  Combine `WHERE` clause with IN and other useful SQL keywords to delete efficiently at once:

``` SQL
DELETE FROM customers
WHERE name IN ('John', 'Alice', 'Bob');
```


Now, we come to the most important aspects of deletion : 
### DELETION WHEN FOREIGN KEY IS INVOLVED
Let me first state the rule directly and then take us through the examples : 

If a FOREIGN KEY exists : `Parent Table -> Child Table` scenario is always there
Then : 

``` Rule
Delete Child first  
Delete Parent later
```

Now, let's explore this in depth why?

Consider the tables : 

![[Pasted image 20260609122108.png|523]]

Now here, the relationship is clearly : 

``` 
artists (Parent)
      ↑
      |
created (Child)
```

#Question  What the foreign key says?

Every value in `created.artist_id` must correspond to an existing `artists.id`. So if
**artists** :

| id  | name                |
| --- | ------------------- |
| 3   | Unidentified artist |
**created** :

| artist_id | collection_id |
| --------- | ------------- |
| 3         | 1             |
then `created.artist_id = 3` is valid because `artists.id = 3` exists.

#Question  Why deleting the parent table value fails?

This is because in that case the child table will have nothing to reference to or basically we will loose the pointer (think in programming terms). The child table record will eventually start to point towards the garbage value or so. Thus, by nature we cannot delete the parent record first in SQL.
So, in the above scenario this is what we can do : 

``` SQL
DELETE FROM "created"
WHERE "artist_id" = (
    SELECT "id"
    FROM "artists"
    WHERE "name" = 'Unidentified artist'
);
```

and then,

``` SQL
DELETE FROM "artists"
WHERE "name" = 'Unidentified artist';
```

#Todo 
But, to combat this tedious process, there also exists something called as : **CASCADE**
or **CASCADE DELETE**


# UPDATE
The `UPDATE` statement is used to update or modify one or more records in a table.

#Syntax 

``` SQL
UPDATE table_name  
SET column1 = value1, column2 = value2, -- ADD MOREd IF NEEDED...  
WHERE condition;
```

**NOTE** : Be careful when updating records in a table! Notice the `WHERE` clause in the `UPDATE` statement. The `WHERE` clause specifies which record(s) that should be updated. If you by mistake omit the `WHERE` clause, all records in the table will be updated!

#Example 

``` SQL
UPDATE Customers  
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'  
WHERE CustomerID = 1;
```

**Updating Single vs Multiple Records**
UPDATE Statement works in such a way that the no. of records that will be updated will depend on the `WHERE` condition. Thus, carefully craft your UPDATE query.

``` SQL
UPDATE Customers  
SET ContactName='Juan'  
WHERE Country='Mexico';
```

In the above query, all the customers with the country as Mexico will have their `ContactName` as 'Juan'.
And this query : 
``` SQL
UPDATE Customers  
SET ContactName='Juan';
```
will update the `ContactName` to "Juan" for ALL records.

And obviously we can have **Nested Queries** in combination with UPDATE Statement.

#Example 

Suppose you want to give a 10% raise to all employees in the HR department, then :
``` SQL
UPDATE employees
SET salary = salary * 1.10
WHERE department_id = (
    SELECT id
    FROM departments
    WHERE department_name = 'HR'
);
```
OR,
another example : Setting customers of a city inactive after that city has become inactive
``` SQL
UPDATE customers
SET active = FALSE
WHERE city_id IN (
    SELECT id
    FROM cities
    WHERE status = 'Inactive'
);
```




