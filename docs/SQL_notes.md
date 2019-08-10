

# MySQL Assignment 1 (due: 7 Aug 9am EST)

## 1.1 MySQL Installation and Database Fundamentals

### 1.1.1 MySQL Installation

Installed: MySQL 8.0.17.

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

### 1.2.5 GROUP BY

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



### 1.2.6 ORDER BY

```mysql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```



### 1.2.7 Functions

Recall Lesson 8, Chinese textbook.



## Exercises 1

### 1 Find duplicate email addresses

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



### 2 Find 'super' countries

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



# MySQL Assignment 2 (due: 11 Aug 9am EST)

## 2.1 MySQL Fundamentals II - Table Operations

### 2.1.1 MySQL Table Data Type

MySQL supports all SQL data types, details can be found in [w3schools](https://www.w3schools.com/sql/sql_datatypes.asp) and [runoob](http://www.runoob.com/mysql/mysql-data-types.html).

### 2.1.2 Create Tables with SQL Statements

Syntax:

```mysql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
   ....
);
```

We can also create tables using another table:

```mysql
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```

SQL constrains can be defined when creating a table. Here are some commonly used constrains:

* NOT NULL: this column cannot have a NULL value.
* UNIQUE: values in this column must be different from each other.
* PRIMARY KEY: set this column as a primary key column. Note: this can also be done with NOT NULL and UNIQUE.
* FOREIGN KEY: uniquely identifies a row in another table.
* CHECK: ensures that all values in a column satisfies a specific condition.
* DEFAULT: sets a default value for a column if no value is assigned.
* INDEX: used to create and retrieve data from the database very quickly.

### 2.1.3 Insert Data into Tables

Two ways:

Specify column names explicitly that need to insert into.

```mysql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

For the other way, there is no need to specify column names if we want to insert values into all columns.

```mysql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

### 2.1.4 Delete Tables

```mysql
DROP TABLE table_name;
```

By executing above statement, the whole table will be permanently deleted and there is NO way around.

```mysql
DELETE FROM table_name WHERE condition;
```

The `DELETE` statement is used to delete existing records in a table. All records that meet the condition will be deleted from the table.

```mysql
TRUNCATE TABLE table_name;  
```

The TRUNCATE statement is used to delete all data in a table, but keep the structure of the table. In other words, the table is still there, but its data is gone, the table is empty with only the column names.

### 2.1.5 Make Changes to Tables

[This post](https://www.javatpoint.com/mysql-alter-table) well describes modifications to a table.



## Exercises 2

### 3 Find classes with more than 5 students

```mysql
CREATE TABLE courses (
	student	varchar(20),
    class varchar(30)
);

insert into courses
values 
	('A', 'Math'),
    ('B', 'English'),
    ('C', 'Math'),
    ('D', 'Biology'),
    ('E', 'Math'),
    ('F', 'Computer'),
    ('G', 'Math'),
    ('H', 'Math'),
    ('I', 'Math'),
    ('A', 'Math');
    
# Solution 1: create a temp table that count number of students in each class
# then find the classes with more than 5 students
select class
from 
	(SELECT count(distinct(student)) as num, class
	FROM courses
	group by class) as temp
where num > 5;

# Solution 2:
SELECT
    class
FROM
    courses
GROUP BY class
HAVING COUNT(DISTINCT student) > 5
;
```



### 4 Switch salary

```mysql
create table salary (
	id int primary key not null,
    name char not null,
    sex char not null,
    salary int not null
);

UPDATE salary
SET 
	sex = 
    CASE sex
		when 'm' then 'f'
		else 'm'
	END;
```



### 5 Interesting movies

```mysql
CREATE TABLE cinema (
	id INT PRIMARY KEY NOT NULL, 
	movie VARCHAR(30) NOT NULL, 
	description VARCHAR(128) NOT NULL, 
	rating FLOAT(10,1)
);

INSERT INTO cinema
VALUES
	(1, 'War', 'great 3D', 8.9),
	(2, 'Science', 'fiction', 8.5),
	(3, 'Irish', 'boring', 6.2),
	(4, 'Ice song', 'Fantacy', 8.6),
	(5, 'House card', 'Interesting', 9.1);

# Solution
SELECT *
FROM cinema
WHERE 
	description <> 'boring'
    AND id % 2 <> 0
ORDER BY rating DESC;
```



## 2.2 MySQL Fundamentals III - Table Connections

### 2.2.1 MySQL Alias

For columns:

```mysql
SELECT column_name AS alias_name
FROM table_name;
```

For Tables:

```mysql
SELECT column_name(s)
FROM table_name AS alias_name;
```

### 2.2.2 MySQL JOIN

`JOIN`s are used with `SELECT` statement. It is performed when you need to fetch records from multiple tables.

Types of MySQL `JOIN`s:

* INNER JOIN, aka simple join
* LEFT JOIN
* RIGHT JOIN
* CROSS JOIN

##### INNER JOIN

Returns all rows from multiple tables where the join condition is met.

```mysql
SELECT columns  
FROM table1   
INNER JOIN table2  
ON table1.column = table2.column;  
```

For example, 2 tables:

Loan:

| LoanNo | Branch | Amount |
| ------ | ------ | ------ |
| 1      | b1     | 1000   |
| 2      | b2     | 2000   |
| 3      | b3     | 1500   |

Borrower:

| name | LoanNo |
| ---- | ------ |
| c1   | 1      |
| c2   | 2      |
| c3   | 4      |

```mysql
SELECT loan.Branch, loan.Amount, borrower.name
FROM loan
INNER JOIN borrower
ON loan.LoanNo = borrower.LoanNo;

# equivalent statement:
SELECT loan.Branch, loan.Amount, borrower.name
FROM loan, borrower
where loan.LoanNo = borrower.LoanNo;
```

Result:

| Branch | Amount | name |
| ------ | ------ | ---- |
| b1     | 1000   | c1   |
| b2     | 2000   | c2   |

Here is the [source video](https://www.youtube.com/watch?v=a-xlrdG-fpY).



##### CROSS JOIN

Take the same example above, if we do not specify the `WHERE` condition in our SQL query, the output will be a cross join of two tables.

```mysql
SELECT loan.Branch, loan.Amount, borrower.name
FROM loan, borrower
```

The resulting number of rows will be the number of rows in table `loan` by number of rows in table `borrower`. This result is obtained without join conditions, it is also known as cartesian product.

| Branch | Amount | name |
| ------ | ------ | ---- |
| b1     | 1000   | c1   |
| b2     | 2000   | c1   |
| b3     | 1500   | c1   |
| b1     | 1000   | c2   |
| b2     | 2000   | c2   |
| b3     | 1500   | c2   |
| b1     | 1000   | c3   |
| b2     | 2000   | c3   |
| b3     | 1500   | c3   |



##### LEFT JOIN

The `LEFT JOIN` keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.

For the same example, we perform `LEFT JOIN`:

```mysql
SELECT loan.Branch, loan.Amount, borrower.name
FROM loan
left JOIN borrower
ON loan.LoanNo = borrower.LoanNo; 
```

Result:

| Branch | Amount | name |
| ------ | ------ | ---- |
| b1     | 1000   | c1   |
| b2     | 2000   | c2   |
| b3     | 1500   | null |



##### SELF JOIN

Syntax:

```mysql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;
```

Join a table with itself. This is useful when one or more records in a table have relationships with other records in the same table. For example let's look at following table `employee-manager`:

| emp_id | first_name | last_name | mgr_id |
| ------ | ---------- | --------- | ------ |
| 1      | Michelle   | Foster    | 162    |
| 162    | Kimberly   | Mendoza   | 191    |
| 191    | Randy      | Spencer   | 193    |

In each record, `emp_id` is this employee's ID number, and `mgr_id` is this employee's manager's ID number. Now we want to make a query to show an employee's name along with his or her manager's name. Here is how we do it by using `SELF JOIN`:

```mysql
select emp.emp_id as employee, emp.first_name, emp.last_name, mgr.emp_id as manager, mgr.first_name, mgr.last_name
from emp_mgr emp, emp_mgr mgr
where emp.mgr_id = mgr.emp_id;
```

Result: 

| employee | first_name | last_name | manager | first_name | last_name |
| -------- | ---------- | --------- | ------- | ---------- | --------- |
| 1        | Michelle   | Foster    | 162     | Kimberly   | Mendoza   |
| 162      | Kimberly   | Mendoza   | 191     | Randy      | Spencer   |

##### UNION

The UNION operator is used to combine the result-set of two or more SELECT statements.

- Each SELECT statement within UNION must have the same number of columns
- The columns must also have similar data types
- The columns in each SELECT statement must also be in the same order

Syntax:

```mysql
# Union
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

# Union all (allows duplicate values)
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

For example, we have an eBay-like website with a database storing item name, cost, and bid. We want to view items with a bid greater than 200 dollars and a cost greater than ​500 dollars, UNION is a nice candidate to solve our question:

```mysql
SELECT name, cost, bids FROM items WHERE bids > 200
UNION ALL
SELECT name, cost, bids FROM items WHERE cost > 500
```



## Exercise 3

### 6 Connect two tables (person and address)

Person:

| PersonId | FirstName | LastName  |
| -------- | --------- | --------- |
| 1        | John      | Green     |
| 2        | Andy      | Black     |
| 3        | Chris     | Jefferson |

Address:

| AddressId | PersonId | city    | state |
| --------- | -------- | ------- | ----- |
| 1         | 1        | Boston  | MA    |
| 2         | 3        | NYC     | NY    |
| 3         | 5        | Buffalo | NY    |

```mysql
select FirstName, LastName, city, state
from person
inner join address
on person.PersonId = address.PersonId;
```

Result:

| FirstName | LastName  | city   | state |
| --------- | --------- | ------ | ----- |
| John      | Green     | Boston | MA    |
| Chris     | Jefferson | NYC    | NY    |



### 7 Delete duplicate email addresses

The solution is based on `SELF JOIN`. We use table `email` twice, one is called `e1` and another is called `e2`. We join them together on the `email` column.

```mysql
select e1.*
from email e1, email e2
where e1.email = e2.email;
```

Result:

| id   | email   |
| ---- | ------- |
| 1    | a@b.com |
| 3    | a@b.com |
| 2    | c@d.com |
| 1    | a@b.com |
| 3    | a@b.com |

Then we want to delete emails with higher id number, as a result we add one more condition:

```mysql
select e1.*
from email e1, email e2
where e1.email = e2.email and e1.id > e2.id;
```

Result:

| id   | email   |
| ---- | ------- |
| 3    | a@b.com |

Now we simply delete this result from the table:

```mysql
delete e1.*
from email e1, email e2
where e1.email = e2.email and e1.id > e2.id;
```



### 8 Customers who never place an order

First we perform a left join to display all customers and their order IDs.

```mysql
Select name as Customers, orders.id
from customers
left join orders
on customers.id = orders.CustomerId;
```

Result:

| Customers | OrderId |
| --------- | ------- |
| Sam       | 1       |
| Joe       | 2       |
| Henry     | null    |
| Max       | null    |

As can be seen that customers who never order has null value in the `OrderId` column. As a result, we add a condition to show only customers whose `OrderId` are null, it will be the final solution.

```mysql
Select name as Customers
from customers
left join orders
on customers.id = orders.CustomerId
where orders.CustomerId is NULL;
```

This [post](https://leetcode.com/articles/customers-who-never-order/) provides another solution:

```mysql
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);
```



### 9 Employees whose salary is higher than their managers

First we list all employee - manager pairs with their corresponding salary values.

```mysql
select emp.name as employee, emp.salary, mgr.name as manager, mgr.salary
from employee emp, employee mgr
where emp.managerid = mgr.id;
```

Result:

| employee | salary | manager | salary |
| -------- | ------ | ------- | ------ |
| Joe      | 70000  | Sam     | 60000  |
| Henry    | 80000  | Max     | 90000  |

Then we add a condition that only shows employees whose salary values are higher than their managers'.

```mysql
select emp.name as employee
from employee emp, employee mgr
where emp.managerid = mgr.id and emp.salary > mgr.salary;
```



## Questions

#### Q: Error 1251 when connecting to localhost database.

![](img\navicat_error.png)

A: Change MySQL authentication method back to `mysql_native_password`, according to [this post](https://blog.csdn.net/seventopalsy/article/details/80195246).



## Notes

1. When creating a table, the names of columns should not be quoted.