# Why databases over spreadsheet software?

There can be multiple reasons for this, but major reasons are : 
1) Scale
2) Frequency
3) Speed

# Database
A collection of data organized for CRUD operations.

# DBMS 
A software via which we can interact with a database. Examples : PostgreSQL, MySQL, SQLite, Oracle, Microsoft SQL Server etc.

Each of these has their own advantages and disadvantages, trade-offs should be considered before making a choice.

# SQL
Stands for Structured Query Language. A language via which we can create, read, update and delete data in a database.

Provides a way to ask questions of data on the database, this process particularly is known as Querying.

#Question :  What questions or what kind of questions can we ask or we may need to ask?

Consider that you are Spotify : Which songs are alike the one that user just played?

Consider that you are Instagram : Which are the most liked posts on our app? Which hashtags are the most used? Which reel is the most trending? or Which reels has most comments? Which comment on Instagram has most likes?

Consider yourself as an consumer product startup founder : You would want to know metrics about user engagement, and that is what can be understood by SQL.

# SELECT
Selects some rows in a table inside of my database. Returns certain rows or maybe all of them if applied in that manner.

#Syntax  

	SELECT COLUMN_NAME FROM TABLE_NAME;  -- when a particular column from a table
	SELECT * FROM TABLE_NAME;  -- when all the columns from a table
	SELECT COLUMN1, COLUMN2 FROM TABLE_NAME;  -- when multiple columns from a table


# LIMIT
Limits the output returned by the query, or the SELECT statement. This is clearly used to regulate the output. Be default it returns from top of the table, which means LIMIT N will return top N results from the table.

#Syntax 

	SELECT COLUMN1 FROM TABLE_NAME LIMIT 5;
	SELECT * FROM TABLE_NAME LIMIT 10;

The order in which the contents (or rows) were added to the table, that is the order in which they will be returned. So we can say that there is a **FIFO** order here.

#Note This LIMIT always is placed at last segment. #Example :

	SELECT title FROM movies ORDER BY title ASC LIMIT 5; ✅
	SELECT title FROM movies LIMIT 5 ORDER BY title; ❌

One more important #Syntax to remember is that OFFSET if always defined after the LIMIT and NOT before it : 

	LIMIT number_of_rows OFFSET number_of_rows_to_skip

Always remember : `Filtering -> Ordering -> Limiting` This is the order of writing SQL query.****

# WHERE
Allows to get back or retrieve NOT all rows but only some rows where some condition is true.
Combined with a condition and the result returned matches the condition or where that condition is true.

#Syntax 

	 SELECT * FROM TABLE_NAME WHERE CONDITION;
	 SELECT COLUMN1, COLUMN2 FROM TABLE_NAME WHERE CONDITION;

Now let's talk about this "condition", for example : In a DB of movies, we can use WHERE clause to filter movies by a particular genre, director, actor, IMDB rating etc.

#Example 

	 SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR = 'Salman';

Now when using WHERE clause, we may have to deal with some operators, so let's study about them.

# OPERATORS
SQL operators are there to help us perform certain set of operations in our query, these could be equality checking, relationship or relative value checking ~ greater than/lesser than, these could be negating an output etc.

= operator : used for equality checking
!= operator : used to check for NOT equal
<> operator : same as above, just another way to write not equal or !=

NOT : negation operator, used to negate a certain result set

#Syntax 

	 SELECT COLUMN FROM TABLE_NAME WHERE NOT CONDITION;

Note that 'NOT' as negation operator is places just after the where and before the condition. Lil different from the semantic of != operator in itself.

#Example 

	SELECT MOVIE_NAME FROM CINEMA WHERE NOT ACTOR = 'Salman';
	SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR != 'Salman';
	SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR <> 'Salman';

All 3 of the above example queries essentially do the same thing.

AND, OR Operator

Same Boolean logic that is applied in programming, that is used over here in SQL as well.

#Syntax 

	SELECT COLUMNS FROM TABLE_NAME WHERE CONDITION1 AND CONDITION2;

We can also pair AND, OR with '()' parenthesis and make more complex conditionals work as well.

#Example 

	SELECT MOVIE_NAME FROM CINEMA WHERE (ACTOR = 'Salman' OR ACTOR = 'Katrina') AND     PRODUCER = 'Yash Raj';


# NULL
Null in SQL is a datatype that signifies nothingness. In non-technical informal language we can attribute that NULL and ZERO are the same thing, but in computer science that's not always the case.

NULL can be used in SQL queries to find out what data is missing in the database.
It can also be **combined with NOT operator** and thus used to figure out what's NOT missing as a counter intuitive function of the NULL only query.

#Example 

	SELECT MOVIE_NAME FROM CINEMA WHERE DUBBING IS NULL;

Consider the above query where we want to find out about the multiple dubbing of movie made. 
See how we figured out here the movies which did not have a dubbing.
And now, using NOT NULL can give us the exact opposite result and help us fine movies where the dubbing is done.

	SELECT MOVIE_NAME FROM CINEMA WHERE DUBBING IS NOT NULL;


# LIKE
Like operator is used for string pattern matching in SQL queries. We cannot every time use '=' sign for matching patterns, in fact the '=' sign only checks for equality and not exactly pattern.

LIKE operator although can be used to find : 
1) A particular kind of word in data at any place
2) A particular kind of word at the start/end

It is almost always paired by 2 characters : % or __ also called as **Wildcards**

Now here we need to know when to use % and when to use __ ?

% : is used when we do NOT know the exact no. of substring in any direction
__ : is used when we want to find according to exact no. of characters in a pattern

#Example 

	SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR LIKE '%Khan%';

Look at the above query - this is a way to find movie names from table CINEMA that will get you the movies with an actor who has 'Khan' in his/her name.

Notice how we have used % sign both before and after, so this will get us names like :
``` SQL
Name "Khan"
"Khan" Surname
Name "Khan" Surname
```

It will basically look for all the matches where name has 'Khan' anywhere
But, consider this query : 

	SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR LIKE '%Khan';

Now this query above will only find movie names where actor has 'Khan' at last in his/her name
like :
'Shah Rukh Khan', 'Salman Khan', 'Amir Khan'
And not like : 

'Khan market', 'Khan Academy'

#Tip  Pairing LIKE with NOT operator can help you find counter intuitive results like movies not done by an actor who has 'Khan' in their names.

#Example 

	SELECT MOVIE_NAME FROM CINEMA WHERE NOT ACTOR LIKE '%Khan';
	SELECT MOVIE_NAME FROM CINEMA WHERE ACTOR NOT LIKE '%Khan';

Both the above queries are **valid**. Placing "NOT" is quite flexible.

#Question What if we want to search movie names who have 'The' in their first character, by this i don't mean only 'The' but also words like 'Then', 'There', 'Their' etc ~ words which are either 'The' or have 'The' as a substring.

	SELECT MOVIE_NAME FROM CINEMA WHERE MOVIE_NAME IS LIKE 'The %';

Notice how we have left a space character after 'The' that signifies look for pattern which has 'The' as a substring at the start, the space character signifies any no. of characters after 'The' as a substring, and yes it will also return those results which do NOT have any character after 'The' and only have 'The' as the first word.


# RANGE CONDITIONS
We can use relational operator like syntax in our SQL queries to find certain data that has a range in it.

 Think of the movie release dates, let's assume that each value of this column was a year, so if we want to know the movies released between 2010 and 2014 then we can use such operators.

These involve : >=, <=, >, < 

#Example 

	SELECT MOVIE_NAME, RELEASE_YEAR FROM CINEMA WHERE RELEASE_YEAR >= 2010 AND          RELEASE_YEAR <= 2014;

Besides using these operators, we can also use :

# BETWEEN
SQL keyword used to find values in a certain range, almost always paired with AND/OR.

#Syntax 

	BETWEEN ... AND ...

#Example 

	SELECT MOVIE_NAME, RELEASE_YEAR FROM CINEMA WHERE RELEASE_YEAR BETWEEN 2010 AND     2014;

NOTE : Here both the values on either side of BETWEEN are **INCLUSIVE**. So in the above query we will get movies from 2010 (inclusive) to 2014 (inclusive) or we can say from start of 2010 to end of 2014.


# ORDER BY
SQL keyword used to sort the data based on a particular metric or column basically. 

#Syntax 

	SELECT COLUMN1 FROM TABLE ORDER BY COLUMN2;
	SELECT COLUMNS FROM TABLE WHERE CONDITION ORDER BY COLUMN;

We can also use it alongside other SQL keywords to make a more complex query to get more in-depth analytical data. 

#Example 

	SELECT COLUMN FROM TABLE WHERE CONDITION BETWEEN THIS AND THAT ORDER BY COLUMN       LIMIT N;

In the above example you can see we have applied a condition on our query and then ordered the result according to a column and also limited the no. of outcomes (or rows).

#Example  Find out the top 10 movies in the table CINEMA

	SELECT MOVIE_NAME FROM CINEMA ORDER BY RATING LIMIT 10;

❌ This is PROBLEMATIC!!

Note : Now the above query will NOT fetch us the data of top 10 movies, the problem here is that ORDER BY works by default in ASCENDING MODE and thus we will get movies with ratings from least to 10th last OR in other words NOT top 10 exactly but last 10 because the default nature of ORDER BY is ASCENDING and lower ratings comes first.

Fix : We have to think this properly when do we want what and how the system of that metric is built, so more points means highly rated and less points means lease rated. This could be different in some other system like least point means high priority and more points means less priority and then if we would have run the same kind of query of order by to get top 10 priority then the logic behind query would have been right.

So we have to think properly, and most importantly about :
1) How is the metric on which we are ordering is built?
2) What way (ASC/DESC) we need to use to get what we want?

Coming back to the previous example to fix this and actually get the top 10 movies, we will write:

	SELECT MOVIE_NAME FROM CINEMA ORDER BY RATING DESC LIMIT 10;

#Syntax 

	ORDER BY COLUMN ASC  -- By default, no need to write ASC
	ORDER BY COLUMN DESC


## COMPOUND ORDERING
This is one of the most interesting concepts or extension of simple ORDER BY clause.
Consider a situation in your CINEMA table where the rating of 2 movies is same and you want to have a deeper look despite the same rating and know which one was loved more by the audience. So assume there is a column in the same table named as "votes". This column tells the no. of votes made by the audience which played just a little part on deciding the rating for the movie.
So now you want to apply ORDER BY but in compound combination of 2 metrics or columns - RATING and VOTES.

Here is how this is done:

	SELECT MOVIE_NAME, RATING, VOTES FROM CINEMA ORDER BY RATING DESC, VOTES DESC        LIMIT 10;

So, we are going to **use comma** to separate columns after ORDER BY, also we can use and in fact we should use **ASC/DESC** after the column name just for the sanity of understanding.

#Syntax 

	ORDER BY COLUMN1 ASC/DESC, COLUMN2 ASC/DESC;

#Question 
Can we use this ordering on string values?
Yes, we can, ORDER BY ASC sorts string in alphabetical order and ORDER BY DESC sorts them in reverse alphabetical order.


# AGGREGATE FUNCTIONS
These are a bunch of functions provided by SQL in which we can combine many rows of a particular metric or column and then return one single individual value of that. These include:

1) COUNT
2) AVG
3) MIN
4) MAX
5) SUM
6) ROUND
7) DISTINCT

AVG
----
#Example Selecting the AVERAGE rating using AVG function

	SELECT AVG(RATING) FROM CINEMA;

The above query will give us the average of all the values in the ratings column or in other words, the average rating of all the movies in our database.

**Result will look somewhat like :** 
``` SQL
AVG(RATING)
-----------
3.753797923
```

Now what we want here is to make it look more presentable and thus we can maybe use ROUND() function to round the result to N decimal places. This is how it can be done:

	SELECT ROUND(AVG(RATING), 2) FROM CINEMA;

ROUND() function takes 2 arguments - metric or value to round, no. of decimal places to round to

**Result here will look like :**
``` SQL
ROUND(AVG(RATING), 2)
------------------
3.75
```

## Using Aliases
To make things look more organized and pretty or more understandable, we can use aliases
#Example 

	SELECT ROUND(AVG(RATING), 2) AS 'Average Rating' FROM CINEMA;

**Result here will look like :**
``` SQL
Average Rating
------------
3.75
```

## COUNT
Simply count the no. of rows. 

#Syntax To count the no. of rows :

	SELECT COUNT(*) FROM TABLE_NAME;

One key thing to remember : If we do : 

	COUNT(*)

Then we are counting all the rows in the table simply, no matter if some row had a NULL value in one of the columns, that would still be counted. But, if we are applying COUNT() on just a single column like :

	SELECT COUNT(DUBBING) FROM CINEMA;

This will only give us the count of the movies whose dubbing is there and the places which have NULL values will be omitted. See the difference is here we are talking about one particular metric only, and thus there is NO point of counting the NULL value as opposed to counting total rows in which NULL values may be present at some places.
So ultimately we can say that when applied on particular columns, the COUNT() function only considers NOT NULL values.

#Todo #Question What if all the values in a row are NULL then what? 

MAX AND MIN
---
For numerical values it gives what is says - max and min values. But, for string values it returns the alphabetical max or the one word that would come last if all the rows arranged alphabetically and alphabetical min using min function which means first word to appear if all the rows content for the same arranged alphabetically.

#Example 

	SELECT MAX(RATING) FROM CINEMA;
	SELECT MIN(RATING) FROM CINEMA;

#Example FOR Strings :

	SELECT MAX(MOVIE_NAME) FROM CINEMA;

you can expect some output like : Zathura : Space Adventure


# SUM
Simply, used to add up the values, always works with numerical values.

#Example 

	SELECT SUM(RATING) FROM CINEMA;
	SELECT SUM(VOTES) FROM CINEMA;

# DISTINCT
We may have repeat values in our tables in multiple columns, consider this - The CINEMA table has a producer column as well and multiple movies might have the same producer (say Yash Raj Films). What if we want to know total no. of producers who made all these movies present in our database?
Well, the better and more appropriate question would be total "distinct" producers?

So if we don't take this "DISTINCT" thing in account we may land up with wrong stats.

#Example 

	SELECT COUNT(PRODUCERS) FROM CINEMA;

Say the above query gives us 80 as result. But there are actually 40 distinct producers and all of them have produced 2 movies comprising the Database. So how do we get to know the proper answer - we use DISTINCT

#Example 

	SELECT DISTINCT(PRODUCER) FROM CINEMA;

Now, this above query will give us the list of distinct producers, no same values.

But what if we have to get the count of total distinct producers? Simply combine it with COUNT() function!!

	SELECT COUNT(DISTINCT PRODUCER) FROM CINEMA;

----END OF LECTURE----

----
---
