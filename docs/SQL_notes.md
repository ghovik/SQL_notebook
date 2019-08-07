# MySQL Assignment 1 (due: 7 Aug 9am EST)

## 1.1 MySQL Installation and Database Fundamentals

### 1.1.1 MySQL Installation

Installed: MySQL 8.0.17.

## Teach yourself SQL

Create new database (schema) `tysql`: 

```
CREATE SCHEMA `tysql` ;
```



## SQL 必知必会

### 1.1.2 Tools for SQL

installed: Navicat 10.0.11 enterprise, and MySQL Workbench 8.0 CE.

### 1.1.3 Database Fundamentals

* **Database**: A database is a collection of structured data.

* **Relational database**: A relational database is a database composed of tables, which are related to one another within the database.

* 二维表: 

* Rows: a row represents a record or an instance in a table.

* Columns: a column represents a keyword or property of a table.

* Primary Keys: primary keys can be any column that satisfies following conditions:
  * Each row has a unique key value, which cannot be NULL;
  * Values in primary key column cannot be modified;
  * Values of primary keys cannot be reused (i.e. if a row is removed from the table, its primary key value cannot be assigned to a new row).

* Foreign Keys: a foreign key is a column in a child table that references a primary key in the parent table.

### 1.1.4 MySQL DBMS

* **Database**: a collection of structured data.
* **Tables**: a table is a primary storage object for data in a relational DB. A simplest table consists of rows and columns that hold data.
* **Views**: a view is a virtual table. It looks like a table and acts like a table, but it does not require physical storage. A view can contain all rows of a table or select rows from a table. A view can be created from one or many tables.
* **Stored procedures**: groups of related SQL statements or functions that are stored in the database, compiled, and ready to be executed by a database user.



## 1.2 MySQL Fundamentals I - query statements

### 1.2.1 Import [demo](https://www.yiibai.com/mysql/how-to-load-sample-database-into-mysql-database-server.html) database `yiibaidb.sql`

Download and put the database file at, for example, `H:/yiibaidb.sql`.

Connect to localhost MySQL server, use following commands to import the database.

```mysql
CREATE DATABASE IF NOT EXISTS yiibaidb DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
use yiibaidb;
# mysql > Database changed
# import database yiibaidb.sql
source H:/yiibaidb.sql
# this importing operation takes about 1 min to finish
# test result:
select city, phone, country from `offices`;
```

Result:

```
+---------------+------------------+-----------+
| city          | phone            | country   |
+---------------+------------------+-----------+
| San Francisco | +1 650 219 4782  | USA       |
| Boston        | +1 215 837 0825  | USA       |
| NYC           | +1 212 555 3000  | USA       |
| Paris         | +33 14 723 4404  | France    |
| Beijing       | +86 33 224 5000  | China     |
| Sydney        | +61 2 9264 2451  | Australia |
| London        | +44 20 7877 2041 | UK        |
+---------------+------------------+-----------+
```

**Note**: use `` instead of '' to quote.

### 1.2.2 What is SQL? What is MySQL?

**SQL**: Structured Query Language is the standard language used to communicate with a relational database.

**MySQL**: MySQL is an open-source DBMS supported by Oracle.

### 1.2.3 Query Statements

##### SELECT FROM

Retrieve data from table(s).

Syntax:

```mysql
SELECT [ * | ALL | DISTINCT COLUMN1, COLUMN2 ]
FROM TABLE1 [ , TABLE2 ];
```

##### DISTINCT

Keyword `DISTINCT` can be used to remove identical values from result set. This keyword must be used before any column.

##### LIMIT

Keyword `LIMIT` is used to display only the first N rows of the result. For example:

```mysql
SELECT prod_name FROM Products LIMIT 5;
```

Which displays the first 5 product names from the table `Products`.

##### CASE ... END

Syntax:

```mysql
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result
END;
```

When `condition1` is met, it returns `result1` and stops reading remaining conditions. If no condition is met, it returns `result`. If there is no `ELSE` statement, it returns `NULL`.

### 1.2.4 Filtering Statements

##### WHERE

Filters result by certain conditions, e.g.

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

Syntax:

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Note**: The `WHERE` clause is not only used in `SELECT` statement, it is also used in `UPDATE`, `DELETE` statement, etc.!

##### Operators

| Operator | Description                                       |
| -------- | ------------------------------------------------- |
| =        | equal                                             |
| >        | greater than                                      |
| <        | less than                                         |
| >=       | greater than or equal                             |
| <=       | less than or equal                                |
| <>       | unequal. **Note**: written as != in some versions |
| BETWEEN  | between a certain range                           |
| LIKE     | search for a pattern                              |
| IN       | to specify multiple possible values for a column  |

A nice example of `IN` can be found [here](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_in).

##### Wildcards

* The percent sign (%): represents zero, one, or multiple characters.
* The underscore (_): represents a single number or character.

Examples:

````mysql
# To find any values that start with 200:
WHERE SALARY LIKE ‘200%’
#To find any values that have 200 in any position:
WHERE SALARY LIKE ‘%200%’
````

More examples can be found in page 124 of the textbook.

More wildcards can be found in [this page](https://www.w3schools.com/sql/sql_wildcards.asp).

##### Logical operators

Page 130

### 1.2.5 1.2.6 GROUP BY

The GROUP BY statement group rows that have the same values into summary rows, like "find the number of customers in each country".

The GROUP BY statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG) to group the result-set by one or more columns.

Syntax:

```mysql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

### 1.2.7 Functions

Recall Lesson 8, Chinese textbook.



## Exercises

### 1. Find duplicate email addresses

Create table `email`, and insert following data:

```
+----+---------+

| Id | Email   |

+----+---------+

| 1  | a@b.com |

| 2  | c@d.com |

| 3  | a@b.com |

+----+---------+
```

Recap syntax:

```mysql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);
```

```mysql
CREATE DATABASE ex1;

CREATE TABLE email (
    id int NOT NULL PRIMARY KEY,
    email varchar(255)
);

INSERT INTO email
VALUES 
	('1', 'a@b.com'),
	('2', 'c@d.com'),
	('3', 'a@b.com');
	
SELECT email
FROM email
GROUP BY email
HAVING COUNT(email) > 1;
```

Result:

```
+---------+
| email   |
+---------+
| a@b.com |
+---------+
```





### 2. Find 'super' countries

```mysql
CREATE TABLE World 
	(  name varchar(50),     
	continent varchar(50),     
	area INT,     
	population INT,     
	gdp INT );
	
INSERT INTO World VALUES
	('Afghanistan', 'Asian', 652230, 25500100, 20343000),
	('Albania', 'Europe', 28748, 2831741, 12960000),
	('Algeria', 'Africa', 2381741, 37100000, 188681000),
	('Andorra', 'Europe', 468, 78115, 3712000),
	('Angola', 'Africa', 1246700, 20609294, 100990000);
	
SELECT name, population, area
FROM World
WHERE area > 3000000 OR (population > 25000000 AND gdp > 20000000);
```

Result:

```
+-------------+------------+---------+
| name        | population | area    |
+-------------+------------+---------+
| Afghanistan |   25500100 |  652230 |
| Algeria     |   37100000 | 2381741 |
+-------------+------------+---------+
```





## Questions

#### Q: Error 1251 when connecting to localhost database.

![](img\navicat_error.png)

A: Change MySQL authentication method back to `mysql_native_password`, according to [this post](https://blog.csdn.net/seventopalsy/article/details/80195246).