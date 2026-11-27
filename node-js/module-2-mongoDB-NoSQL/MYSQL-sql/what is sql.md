# What is SQL, and how do you use SQL queries?

SQL (Structured Query Language) is a language used to create, read, update, and delete data in relational databases such as MySQL, PostgreSQL, and SQL Server. Data is usually organized into tables made up of rows and columns.

## Where do you write SQL?

Connect to a database using a tool such as MySQL Workbench or the MySQL command-line client, select a database, and run a query. The database executes the query and returns results or reports whether the change succeeded.

## Basic MySQL examples

Select a database:

```sql
USE school;
```

Create a table:

```sql
CREATE TABLE students (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100) NOT NULL,
  age INT
);
```

Add a row:

```sql
INSERT INTO students (name, age)
VALUES ('Asha', 20);
```

Read rows:

```sql
SELECT id, name, age
FROM students;
```

Filter and sort results:

```sql
SELECT name, age
FROM students
WHERE age >= 18
ORDER BY name;
```

Update a row:

```sql
UPDATE students
SET age = 21
WHERE id = 1;
```

Delete a row:

```sql
DELETE FROM students
WHERE id = 1;
```

Use a `WHERE` condition with `UPDATE` and `DELETE` so you change only the rows you intend. Without `WHERE`, those statements affect every row in the table.

## Common SQL commands

- `CREATE` makes a database object, such as a table.
- `INSERT` adds data.
- `SELECT` reads data.
- `UPDATE` changes data.
- `DELETE` removes data.

Most SQL statements end with a semicolon (`;`).


# SQL query are  ?

1. How to create database via SQL ?

**examples**
```
create database db_flipkart
```

2. How to create table via SQL ?

**examples**
```
create table users(

    uid int AUTO_INCREMENT primary key,
    uname varchar(255),
    password varchar(255),
    email varchar(255),
    gender varchar(255),
    mobile bigint,
    address text

)

or

create table feedback(

    feedid int AUTO_INCREMENT primary key,
    name varchar(255),
    email varchar(255),
    mobile bigint,
    rating varchar(255),
    comments text

)

or 

create table customers(

    custid int AUTO_INCREMENT primary key,
    name varchar(255),
    email varchar(255),
    password varchar(255),
    mobile bigint,
    country varchar(255),
    state varchar(255),
    city varchar(255)

)
```

3. How to insert data in table 

**examples**

```

insert into users(uname,password, email, gender, mobile, address) values('amish','a123','amish@gmail.com','male',945212121,'150 feet ring road rajkot')

or 

insert into users(uname,password, email, gender, mobile, address) values('mayur','m123','mayur@gmail.com','male',945214578,'150 feet ring road rajkot'),('mahiraj','m123','mahiraj@gmail.com','male',94521416545,'150 feet ring road rajkot'),('twinkle','t123','twinkle@gmail.com','female',65216545,'150 feet ring road rajkot')

or 

insert into users values(null,'om','o123','om@gmail.com','male',945214578,'150 feet ring road rajkot'),(null,'amish','a123','amish@gmail.com','male',94521416545,'150 feet ring road rajkot'),(null,'rutwick','r123','rutwick@gmail.com','male',65216545,'150 feet ring road rajkot')

```

# how to update data in tables ? 

 **update query**

 ```
 update users set uname='rushiraj',password='rushi123',email='rushiraj@gmail.com',gender='male',mobile=61223121,address='near CG road ahemdabad' where uname='rutwick';

 or 

 update users set uname='rushiraj',password='rushi123',email='rushiraj@gmail.com',gender='male',mobile=61223121,address='near CG road ahemdabad' where uid=7;

 ```

 # how to delete data from tables via SQL ? 

 **examples**

 ```
 delete from users;
 or
 delete from users where uid=2;
 or 
 delete from users where uname='rushiraj';
 or 
 delete from users where uid in(2,3,4);
 or 
 delete from users where uid between 1 and 5;

 ```


# SQL functions ? 
1. sql function are inbuilt function 

# types of SQL function 

1. aggrigate function
   - max()
   - min()
   - avg()
   - sum()
   - count()
2. scalar function

   - first()
   - last()
   - lcase()
   - ucase()

**examples of sql function**

1. select max(salary) as max_salary from employee;
2. select min(salary) as min_salary from employee;
3. select avg(salary) as avg_salary from employee;
4. select sum(salary) as sum_salary from employee;
5. select count(empid) as total_employees from employee;
6. select first(empid) from employee;
7. select last(empid) from employee;
8. select lcase(name) from employee;
9. select ucase(name) from employee;


# sql subquery : 

 1. query within another query i.e called subquery
 2. subquery should multiple 

**examples of subquery**

```
select max(salary) as second_highest_salary from employee where salary < (select max(salary) from employee)

```

# difference b/w order by & group by 

# order by :
1. order by filter data in acsending order and desending order

**examples**

```
select * from employee order by name asc;
or
select * from employee order by name desc;

```

# group by :

1. filter data on group of columns there we applied group by  

**examples**

```
select sum(salary), department from employee group by department;
```

# group by with having clause ?

1. having clause always work after group by 
2. having does not work before group and after where

**examples**

```
select sum(salary), department from employee group by department HAVING AVG(age) > 25;
or
SELECT SUM(salary) AS total_salary, department
FROM employee
GROUP BY department
HAVING AVG(age) > 25;
```

# key constraints in SQL :

1. set limitation on sql via provide  pk | fk | uk 
2. pk is auto_increment and set once time in a tables 
3. fk is used to provides relationship b/w tables
4. uk is used to never return a duplicate values

# how create pk ? 

1. A pk never return a null values 
2. A pk key should be always auto_increment
3. A pk always assign once time in a tables 

```
create table country
(
cid int AUTO_INCREMENT primary key,
cname varchar(255)    
)
```


# how create uk ? 

1. A uk  return once time  null values 
2. A uk never return or stored a duplicate data
3. A uk assign more than once  time in a tables 


```
create table customers
(
custid int AUTO_INCREMENT primary key,
name varchar(255),
email varchar(255),
mobile bigint,
address text    
)

or

alter table customers add unique key(`email`)

```

# how create fk ? 

1. A fk is used for provides relationship in tables  
2. A fk stored a duplicate data
3. A fk assign more than once  time in a tables 
4. A fk is used to provides relationship b/w tables with common field used for join tables 

```
create table state
(
sid int AUTO_INCREMENT primary key,
sname varchar(255),
cid int,    
CONSTRAINT fk_state_country    
FOREIGN key(cid) REFERENCES country(cid)     
)

or

create table city
(
ctid int AUTO_INCREMENT primary key,
ctname varchar(255),
sid int,    
CONSTRAINT fk_city_state    
FOREIGN key(sid) REFERENCES state(sid)     
)

or

create table users
(
uid int AUTO_INCREMENT primary key,
name varchar(255),
email varchar(255),
password varchar(255),
mobile bigint,
    
cid int,    
CONSTRAINT fk_country    
FOREIGN key(cid) REFERENCES country(cid),         
    
sid int,    
CONSTRAINT fk_state    
FOREIGN key(sid) REFERENCES state(sid),
    

ctid int,    
CONSTRAINT fk_city    
FOREIGN key(ctid) REFERENCES city(ctid),
    
address text
)


```

# SQL join ? 

1. sql join are used to join more than one tables with common field if data matched from one tables to another tables join the data.

2. SQL join are used to joined matched data from one tables to another tables 

# types of join ? 

1. join 
2. inner join 
3. outer join 
   - left join 
   - right join 
   - full join 
4. cross join 
5. union join 

**examples of join**

```

select users.*, cname , sname , ctname from users join country on users.cid=country.cid join state on users.sid=state.sid join city on users.ctid=city.ctid;

```