---
title: "SQL Notes (1)"
date: 2020-05-04T00:10:29-07:00
draft: false
toc: false
images:
tags:
- Notes
- SQL
categories:	
- Data Science
- Database
---

#### Useful Commands

`DESCRIBE`: to look at how the table is constructed. This shows if a column is a primary key and what type of data is being srored in each column. 



#### Database level：

A. representation of all the structures, such as tables and colimns, in your database, along with how they connect, is known as **schema**. 

```SQL
CREATE DATABASE test;
DROP DATABASE test;
SHOW DATABASE; -- show all the databases
USE test; -- use some specific database
```

#### In a Database:

The **foreign** key is a column in a table that reference the **primary** key of another table. 

```SQL
SHOW TABLE;

/*If you want to modify or update a certain table*/
ALTER TABLE table_name;
ADD column_name datatype;
DROP COLUMN column_name;
SELECT DISTINCT col1,col2 FROM table_name; -- A col may contain duplicate values
SELECT ... FROM... WHERE conditions; -- filter the rows returned from FROM
```

##### COUNT

```SQL
SELECT COUNT(*) FROM table_name; -- In real life, * is not a good idea.
SELECT COUNT(col) FROM table_name;
SELECT COUNT(DISTINCT col) FROM ... ;
```

```SQL
/*LIMIT is useful when want to get all cols but not all rows*/
LIMIT 5; -- 5 rows and all cols

/* ORDER BY: sorting */
SELECT... FROM... ORDER BY col1 ASC/DESC 

ROUND(...,2); -- round to 2 decimal places
```

##### BETWEEN, IN, LIKE

```SQL
value BETWEEN low AND high
value IN(SELECT value FROM ... );-- here is a subquery

SELECT ... FROM ... 
WHERE firstname LIKE 'Jen%';-- % pettern matching sequence.
		...		LIKE 'JEN_';-- _ pattern matching single char.
```

##### Aggregate Function:

min, max, avg, sum, count,... which take a lot rows of data and return a single value.

##### GROUP BY:

Which group by clause devides the rows returned from SELECT into groups

``` SQL
SELECT... FROM... GROUP BY col;
SELECT customer_id, SUM(amount)
FROM payment
GROUP BY costumoer_id
ORDER BY SUM(amount) DESC

SELECT col1, some_aggr_fun(col2)
FROM table_name
WHERE ...
GROUP BY col1
HAVING (conditions) -- ★★ The condition here is related to the aggr_fun to the first line. ★★
```

##### WHERE v.s. HAVING ★★★

- WHERE: 
  - more cheapter and efficient. 
  - applied when read data from table 
  - there are a lot of optimization techniques, such as indexes, to skip a bunch of rows.

- HAVING:
  - **Huge** resource consumption 
  - **used to filter data after aggregation function.**
  - usually applied **after `GROUP BY`.**

##### JOIN

`JOIN` means `INNER JOIN` in SQL.

![](https://i.stack.imgur.com/VQ5XP.png)

##### UNION

Combine result sets of two or more SELECT statements into a single result set.

```SQL
SELECT co11, col2
FROM table1
UNION
SELECT col3,col4
FROM table2
```

##### TIMESTAMP and EXTRACT

`SELECT EXTRACT (DAY FROM payment_date);`

##### CREATE DATABASE and TABLES

```SQL
CREATE TABLE table_name(col_name1 data_type PRIMARY KEY, -- this contraint is combination of NOT NULL and UNIQUE
                        col_name2 data_type,
                          ... )
```

##### CONSTRAINTS:

```SQL

CHECK -- enables to check a condition when you insert or update a data
NOT NULL -- the value of the table cannot be null
UNIQUE --  the value of the table must be unique across the whole table
PRIMARY KEY -- must be NOT NULL and UNIQUE
FOREIGN KEY -- provides a link between data in two tables. it points to a PRIMARY KEY in another table.
REFERENCES -- constr. the value of the col that exists in a col in another table.
```



![](https://i.ytimg.com/vi/Osv7AhGq_Vc/maxresdefault.jpg)

```SQL
CREATE TABLE table_name 
(col_name TYPE col_constraint, table_constraint)
INHERITS exsiting_table_name
```



##### INSERT

```SQL
INSERT INTO table_name (col1,col2,...)
VALUES (value1,value2,...)

/* insert multiple rows: */
INSERT INTO table_name(col1,...)
VALUES(val1,val2,...),
		(val3,val4,...),...

/* insert from other table */
INSERT INTO table_name
SELECT col1
FROM another_table
WHERE condition
```



##### UPDATE, DELETE

```SQL
UPDATE table_name
SET col1 = val1,col2  = val2, ...
WHERE condition

DELETE FROM table_name
WHERE condition
```

##### ALTER/DROP TABLE

```SQL
ALTER TABLE table_name -- a series of actions
DROP TABLE table_name
```



Quick Links:

[SQL Notes (2)](https://ffflora.cat/posts/2020/05/sql-notes-2/)

[SQL Notes (3)](https://ffflora.cat/posts/2020/05/sql-notes-3/)
