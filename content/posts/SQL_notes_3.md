---
title: "SQL Notes (3)"
date: 2021-12-06T21:43:17-08:00
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

### Smart Table Design

##### Here are some very broad steps for creating new tables:

1. Pick **the one thing** you want your table to describe.
2. Make a list of the information you need to know about that one thing when you are using the table.
3. Using the list, **break down the information** about your thing into **pieces** you can use for **organizing** your table.

##### And here are some rules of atomic data, which should be the most important part of your table. 

1. A column with atomic data can't have several values of the same type of data in that column.
2. A table with atomic data can't have multiple columns withe same type of data.

#### Creating Tables

##### Normal Table 

Making the data atomic is the first step in creating a normal table, since it - 

1. won't have duplicate data, which will reduce the size of DB
2. has faster queries as it has less data.

##### 1NF - First Nornal Form

1. Columns contain only atomic values. 
2. No repeating groups of data.

##### 2NF - Second Nornal Form 

1. Be in 1NF
2. Have no partial functional dependency. 

##### 3NF 

1. Be in 2NF
2. Have no transitive dependencies

##### Terminology

Each row of data must have a unique identifier, known as a **Primary Key**.

A **COMPOSITE KEY** is a **primary** key composed of multiple columns, creating a unique key. 

A **partial functional dependency** means that a **non-key** column is dependent **on some, but not all,** of the columns in a composite primary key. 

If changing any of the non-key columns might cause any of the other columns to changem you have a **transitive dependency.** 

You can review and (potentially reuse) the code to recreate the current table by 

``` SQL
SHOW CREATE TABLE m_table;
```

##### CREATE TABLE with a PRIMARY KEY

```SQL
CREATE TABLE my_contacts
(
	contact_id INT NOT NULL,
  aaa VARCHAR(30) default NULL,
  bbb char(1) default NULL,
  PRIMARY KEY (contact_id)
);

--------------------------------
CREATE TABLE my_contacts
(
	contact_id INT NOT NULL AUTO_INCREMENT, -- goes up by 1 autometically
  aaa VARCHAR(30) default NULL,
  bbb char(1) default NULL,
  PRIMARY KEY (contact_id)
);
```

##### Create a table with a FOREIGN KEY

```SQL
CREATE TABLE interests(
int_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  interest VARCHAR(50) NOT NULL,
  contact_id INT NOT NULL,
  CONSTRAINT my_contact_id 
  FOREIGN KEY(contact_id)
  REFERENCES my_contacts(contact_id)
)
```



##### Adding a PRIMARY KEY to an existing table - ALTER

```SQL
ALTER TABLE my_table
ADD COLUMN contact_id INT NOT NULL AUTO_INCREMENT FIRST, -- FIRST makes the column goes first in the table
ADD PRIMARY KEY(contact_id);
```



#### Table Maintenance 

- Make changes to the created table: `ALTER TABLE`. 

- Change orders of the columns with the keywords `FIRST`, `AFTER`, `SECOND`, Nth, `LAST` and etc.

  Note: if you use `ALTER` to change a column of one data type to another type, there's a risk losing the data.

- Delete table: `DROP TABLE`

- `DROP COLUMN` - delete a column from table 

- `ADD` - add a column to the table, the data type can be picked.

- `MODIFY` the **only** data type or position of an existing column.

- `CHANGE` **both** name and data type of an existing column.

- `RENAME` the column name.

- ```SQL
  ALTER TABLE Vendors
  ADD vend_phone CHAR(20);
  AFTER first_name -- indicates the position
  
  ALTER TABLE Vendors
  DROP COLUMN vend_phone;
  
  ALTER TABLE Vendors
  RENAME TO VendorsList; -- renaming the table
  
  /*Update the column name from number -> project_id, and same to the other. You can perform many column changes in one SQL statement*/
  ALTER TABLE Projects
  CHANGE COLUMN number project_id INT NOT NULL AUTO_INCREMENT,
  CHANGE COLUMN cont contractor VARCHAR(30) NOT NULL AUTO_INCREMENT,
  ADD PRIMARY KEY(project_id)
  
  ALTER TABLE project_list
  MODIFY COLUMN vendors VARCHAR(100)
  
  ALTER TABLE project_list
  DROP COLUMN vendors
  --------------------------------
  DROP TABLE CustCopy;
  ```

  

### Multi-table database design

#### When t use one-to-one tables

It generally makes more sense to leave one-to-one data in your main table, but there are a few advantages you can gt from pulling those columns out at times:

1. Pulling the data out may allow you to write faster queries.
2. If you have a column containing values you don't yet know, you can isolate it and avoid NULL in your main table.
3. You may wish to make some of your data less accessible.
4. If you have a large piece of data.

Many-to-Many: a junction table holds a key from each table.

```SQL
/*Three ways to the same end*/
/*1.	CREATE TABLE, then INSERT with SELECT. */
CREATE TABLE profession(
	id INT(11) NOT NULL SUTO_INCREMENT PRIMARY KEY,
  profession VARCHAR(20)
);
INSERT INTO peofession(profession)
	SELECT profession FROM my_contacts
	GROUP BY profession
	ORDER BY profession
--------------------------------
/*2.	CREATE TABLE with SELECT, then ALTER to add primary key */
--------------------------------
```

