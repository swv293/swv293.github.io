---
layout: post
title: How to calculate median using PostGRESQL?
---
### The many ways of constructing a SQL query

![DVD](/images/blog4/people-buy-sell-cassettes.jpg)
The Dell DVD store is a fabulous online resource to test SQL skills

Photo from Pexels

SQL or Structured Query Language has emerged as a critical requisite for data scientists to know in addition to statistics, and machine learning algorithms. And that is rightfully so. For a data scientist to work his/her 'magic', he/she had to access data. Most businesses tend to store their data in relational databases - and SQL just happens to be an efficient means to get the data we want!

>What is a relational database?

>A database based on the [Relational Model](https://en.wikipedia.org/wiki/Relational_model) of database management created by [Edgar F. Codd](https://en.wikipedia.org/wiki/Edgar_F._Codd), based on the mathematical foundations of set theory and logic. With this background, relational databases store data in the form of tables that have various fields (columns), containing elements of the same kind and records (rows), which are collections of different fields for one individual entry. Data stored in these tables are usually related to one another through selected fields (foreign keys) and are separated to minimize duplication and store data efficiently. The major advantage of the relational databases is the ease of data retrieval with SQL queries, which utilize the underlying mathematics of the database model to return the data that you want.

It is a good idea to learn SQL - which is fairly logical to follow given its underpinnings (see above) - from a number of good resources found through a google search (Try [W3 Schools](https://www.w3schools.com/sql/sql_intro.asp) for a start)! And once your basics are in place, it is great if you had access to a relational table for you to test and exhibit your SQL skill-set.

### The Dell DVD store

The Dell DVD store is a great open resource for a relational database of an online e-commerce website. The set of 8 relational tables comes with codes to implement any one of the different flavors of SQL. In my case, I chose to implement it in PostGreSQL on an AWS instance, and query it either directly in the psql implementation on the instance or through a Jupyter notebook using the psycopg2 PostGRESQL adapter for python. For more details on how to setup an AWS instance with the Dell DVD store data check out my [GitHub repo](https://github.com/swv293/Dell_DVDStore_SQL).

Once you set it up, you can test your SQL skills by asking interesting questions! However, for this particular blog post, I focus how to calculate the median of a distribution using SQL, in particular the myriad ways of creating a median SQL query!

### The missing aggregator?

SQL has some basic aggregation methods available - `MIN`, `MAX`, `COUNT`, `AVG` and `SUM` with which you can summarize most stats, but there is nothing set up specifically for calculating the median.

>__Median__
>The median is defined as the quantity lying at the middle of a frequency distribution of ordered values. Or in other word, it is the middle value that separates the upper half from the lower half of the data.
> So for a distribution of numbers like (1,3,3,5,8,9,10) the median would be 5. However, if we had an even number of values like (1,3,3,5,8,9), the mean would be the average of 3 and 5 which is 3 + 5 = 8/2 = 4.

So one of the crude ways to calculate the median for a field in SQL is to order values in ascending order and select the top 50% of the values (plus 1, if the total number of values is even). This list can be then reversed (ordered in descending order) and can either select the top value (for odd number of values) or select the average of the top two (for even number of values).

I have used the total amount in the orders table as an example to highlight. Here the crude SQL sample query:

`SELECT AVG(totalamount) as median
FROM
(SELECT *
FROM
(SELECT totalamount
FROM orders
ORDER BY totalamount
LIMIT 12000/2+1) as q1
ORDER BY totalamount DESC
LIMIT 2) as q2`

This gives us the median value of the amount purchased in every order - $219.54

PostGRESQL offers us a few other ways to calculate the median, of which we will try three other methods.

#### `OFFSET` and `FETCH`

In a manner similar to the method used above, we can use the `OFFSET` method to omit a certain number of rows and `FETCH` the `NEXT` x rows. So for the median we omit the first 5999 rows and select the next 2 rows in case of an even number of rows (or next one in case of odd).

`SELECT AVG(totalamount)
FROM (SELECT totalamount FROM orders
ORDER BY totalamount
OFFSET 5999 ROWS
FETCH NEXT 2 ROWS ONLY
) AS x`

#### `ROW_NUMBER`

This is an interesting approach where you create a subquery with the total amount, a field of row numbers for all rows in the ascending order and another for row numbers in the descending order. The main query selects the average of the fields where the ascending row number is between the corresponding value of the descending row number +/- 1. This would result in the selection of either the middle value for odd row numbers or the two middle rows for an even number of rows. The SQL query is listed below:

`SELECT ROUND(AVG(totalamount), 2) as median_totalamount`  
`FROM`  
`(SELECT totalamount,`
`ROW_NUMBER() OVER (ORDER BY totalamount) AS rows_ascending,`
`ROW_NUMBER() OVER (ORDER BY totalamount DESC) AS rows_descending`
`FROM orders`
`WHERE totalamount <> 0`
`) AS x`
`WHERE rows_ascending BETWEEN rows_descending - 1 AND rows_descending + 1`

#### `PERCENTILE_CONT`

Introduced in SQL server version in 2012, and incorporated into PostgreSQL since 2014, this is a nifty way to calculate the median. It calculates the 50th percentile (which is the median), as a continuous function (the `_CONT` part). What that means is that it will interpolate in case there are multiple values. Now, `PERCENTILE_CONT` will calculate the median in the context of a grouping (the `WITHIN GROUP` part), which is used to order the list in an ascending manner. If there were further partitions that needed then you could use the window functions - `OVER` and `PARTITION`.

`SELECT  
ROUND(PERCENTILE_CONT(0.50) WITHIN GROUP (ORDER BY totalamount)::numeric, 2)
AS median_totalamount
FROM orders`

In case you were wondering what the `::numeric` did - it is the same as `CAST` - it changes the data type to numeric so that `ROUND` can do its magic!

### So, in conclusion...

We took a look at the different ways in which median can be calculated. However, it is important to keep the performance in mind. Not all SQL queries are equally efficient when dealing with huge amounts of data. The `OFFSET`/`FETCH` method is supposed to be more efficient than most methods, with the worst performer being `PERCENTILE_CONT` (see this [blog post](https://sqlperformance.com/2012/08/t-sql-queries/median) for performance comparisons)!

Happy SQL learning!

### Tools used for the project

* AWS EC2
* PostGRESQL
* Python and psycopg
* Jupyter Notebook
* Bash scripting
* Command line interface (CLI)

Check out [my GitHub repo](https://github.com/swv293/Dell_DVDStore_SQL) for more details regarding the project, codes and results!
