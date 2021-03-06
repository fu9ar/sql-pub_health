---
layout: lesson
root: .
title: Basic queries
minutes: 30
---


Let's start by using the **Demographics** table.
Here we have data on general demographics from every county in the United States.

Let’s write an SQL query that selects only the Population_Size column from the Demographics
table.

    SELECT Population_Size FROM Demographics;

We have capitalized the words SELECT and FROM because they are SQL keywords.
SQL is case insensitive, but it helps for readability – good style.

If we want more information, we can just add a new column to the list of fields,
right after SELECT:

    SELECT State, County, Population_Size FROM Demographics;

Or we can select all of the columns in a table using the wildcard *

    SELECT * FROM Demographics;

### Unique values

This isn't terribly useful on this dataset, but we can output a list of distinct values from a column using the SQL Keyword, ``DISTINCT``

    SELECT DISTINCT State FROM Demographics;

If we select more than one column, then the distinct pairs of values are
returned. Note that the Poverty measurements are percentages, not raw numbers.

    SELECT DISTINCT State, Poverty FROM Demographics;
    
Here, you can look at the bottom of the SQLite Manager window, in the status bar and you should see that this returns 2167 records compared to the 3141 returned from

    SELECT * from Demographics;

### Calculated values

We can also do calculations with the values in a query.
For example, if we needed counts of the people living in Poverty in each County

    SELECT County, Population_Size*Poverty/100 from Demographics;

When we run the query, the expression `Population_Size*Poverty/100` is evaluated for each row
and appended to that row, in a new column.  Expressions can use any fields, any
arithmetic operators (+ - * /) and a variety of built-in functions (). For
example, we could round the values to make them easier to read. While you may not need two decimal places when evaluating population demographics, I included it here to illustrate how the function arguments work.

    SELECT County, ROUND(Population_Size*Poverty/100, 2) from Demographics;

> ## Challenge
>
> Write a query that returns the names of the counties with Population Size, and Numbers (not percentages) of White, 
> Black,  Hispanic, and Asian people

Filtering
---------

Databases can also filter data – selecting only the data meeting certain
criteria.  For example, let’s say we only want the data for Massachusetts.  
We need to add a WHERE clause to our query:

    SELECT * FROM Demographics WHERE State='TX';

We can do the same thing with numbers.
Here, we only want the data for counties with more than 20% Poverty:

    SELECT * FROM Demographics WHERE Poverty >= 20;

We can use more sophisticated conditions by combining tests with AND and OR.

    SELECT * FROM Demographics WHERE (Poverty >= 20) AND (State = 'TX');

Note that the parentheses aren’t needed, but again, they help with readability.
They also ensure that the computer combines AND and OR in the way that we
intend.

    SELECT * FROM Demographics WHERE (State = 'TX') AND (State = 'OK');

Whoops! Why didn't that work? SQL uses strict logical operators for these selections.

    SELECT * FROM Demographics WHERE (State = 'TX') OR (State = 'OK');

> ### Challenge
>
> Write a query that returns the counties where Poverty is >= 20 in TX and the surrounding states.


Saving & Exporting queries
--------------------------

* Exporting:  **Actions** button and choosing **Save Result to File**.
* Save: **View** drop down and **Create View**


Saving queries
--------------------------------------
To enable saving queries from the main menu select **Tools** -> **Use Table for Extension Data**:

![Saving queries](img/saving_query.png)

You will see additional 4 icons - "Previous query from the history", "Next query from the history", "Save query by name" and "Clear query history". When you click the query, it will then be available under the list of queries ("Select a Query").

Building more complex queries
-----------------------------

Now, lets combine the above queries to get data for Mass and the surrounding states.  This time, 
let’s use IN as one way to make the query easier to understand.  It is equivalent to 
saying `WHERE (CHSI_State_Abbr = 'MA') OR (CHSI_State_Abbr = 'NH) ...`, but reads more neatly:

    SELECT * FROM Demographics 
    WHERE (Poverty >= 12) 
    AND (CHSI_State_Abbr IN ('TX', 'NM', 'OK', 'LA'));

We started with something simple, then added more clauses one by one, testing
their effects as we went along.  For complex queries, this is a good strategy,
to make sure you are getting what you want.  Sometimes it might help to take a
subset of the data that you can easily see in a temporary database to practice
your queries on before working on a larger or more complicated database.


Sorting
-------

We can also sort the results of our queries by using ORDER BY.
For simplicity, let’s go back to the Demographics table and order it by Poverty rates.

    SELECT * FROM Demographics ORDER BY Poverty ASC;

The keyword ASC tells us to order it in Ascending order.
We could alternately use DESC to get descending order.

    SELECT * FROM Demographics ORDER BY Poverty DESC;

ASC is the default.

We can also sort on several fields at once. This is somewhat of a contrived example, but it illustrates the concept.

    SELECT * FROM Demographics ORDER BY Population_Density ASC, Poverty ASC;

> ### Challenge
>
> Write a query that outputs the counties in Mass and the surrounding states with Poverty >= 12, sorted by
> Population Size.


Order of execution
------------------

Another note for ordering. We don’t actually have to display a column to sort by
it.  For example, let’s say we want to order the counties in Mass by their population, but
we only want to see Poverty and the name of the county.

    SELECT County, Poverty FROM Demographics WHERE State = 'TX' ORDER BY Population_Size ASC;

We can do this because sorting occurs earlier in the computational pipeline than
field selection.

The computer is basically doing this:

1. Filtering rows according to WHERE
2. Sorting results according to ORDER BY
3. Displaying requested columns or expressions.


Order of clauses
----------------

The order of the clauses when we write a query is dictated by SQL: SELECT, FROM, WHERE, ORDER BY
and we often write each of them on their own line for readability.


> ### Challenge
>
> Let's try to combine what we've learned so far in a single
> query.  Using the Demographics table write a query to display 
> counties and poverty levels nationwide with Populations over 5000,
> Population Density over 500, sorted by the percentage of Black residents.




Previous: [SQL Introduction](00-sql-introduction.html) Next: [SQL Aggregation.](02-sql-aggregation.html)
