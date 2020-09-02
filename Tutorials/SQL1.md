![banner_1](https://user-images.githubusercontent.com/37455387/92022008-43f1cd00-ed78-11ea-89f8-07e6e014d681.jpg)

# Structured Query Language for Datascience part1 <br><br>
## Structured Query Language
SQL, Structured Query Language, is a programming language designed to manage data stored in relational databases. 
SQL operates through simple, declarative statements. This keeps data accurate and secure, and helps maintain the integrity of databases, regardless of size.
The SQL language is widely used today across web frameworks and database applications. 
Knowing SQL gives you the freedom to explore your data, and the power to make better decisions. By learning SQL,


### What Can SQL do?


- SQL can execute queries against a database
- SQL can retrieve data from a database
- SQL can insert records in a database
- SQL can update records in a database
- SQL can delete records from a database
- SQL can create new databases
- SQL can create new tables in a database
- SQL can create stored procedures in a database
- SQL can create views in a database
- SQL can set permissions on tables, procedures, and views


### Creating a table


`CREATE TABLE movies (
    id    integer;
    name text,
     genre text,
    year date,
    imbd_rating integer
);`


The following SQL statement selects all the columns from the  table:


`SELECT *
FROM movies; `


![2s](https://user-images.githubusercontent.com/37455387/92023426-6389f500-ed7a-11ea-92ad-2b05e276ec2a.PNG)


### Selecting specific column


`SELECT name
FROM movies; `


![3s](https://user-images.githubusercontent.com/37455387/92023436-65ec4f00-ed7a-11ea-9398-a64caee429ed.PNG)



### Selecting specific column with condition


`SELECT name
FROM movies
WHERE id>10;`


![4s](https://user-images.githubusercontent.com/37455387/92024250-954f8b80-ed7b-11ea-8cb3-b088991737aa.png)


### Inserting new values


`insert into movies 
values(21,Tenet,thrill,2020,8.0);`


![3](https://user-images.githubusercontent.com/37455387/92027293-f5483100-ed7f-11ea-9d5e-a0d3ae57de31.PNG)


### Updating values


`UPDATE movies
SET genre = Action/thrill
WHERE id =20;`


![6s](https://user-images.githubusercontent.com/37455387/92026591-e14fff80-ed7e-11ea-961c-8499b3dba4d2.PNG)


### Deleteing record

`DELETE FROM movies WHERE id<10;`


![4s](https://user-images.githubusercontent.com/37455387/92024250-954f8b80-ed7b-11ea-8cb3-b088991737aa.png)


![Travolta-memes](https://user-images.githubusercontent.com/37455387/92022029-494f1780-ed78-11ea-82e8-813e86c978c3.jpg)

References:

[CODEACEDMY](https://www.codecademy.com/paths/data-science/tracks/sql-basics/modules/dspath-sql-queries/lessons/queries/exercises/select-ii)

[W3SCHOOLS](https://www.w3schools.com/sql/)
