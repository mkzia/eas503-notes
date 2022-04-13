# Creating Tables

## Basic
A database is made up of tables. Tables are made up of columns and rows. A database can be considered like an Excel file
and the sheets in your Excel file are like tables. The power of database comes from joining different tables based on
shared column information/name between tables and filtering and aggregating rows. SQL is the language that allows
you to create, update, delete, filter, and aggregate tables. Often the first step in data wrangling is to extract the
data you need from an SQL table. Sometimes you might even have to parse data and populate into a database before you can
start using it. 


## SQLite3 
- SQLite3 supports three primitive data types: INTEGER, REAL, TEXT, and BLOB. 

```{warning}
SQLite3 is dynamically typed. This means that even if you specify a column to be data type real or integer, you can insert
a string type into it. If the string type can be cast into a numeric type, it will be, otherwise it will be left as is. 
If you apply a sum or other math function to a column with mixed data types, only the numerical values will be used. 
```

## Creating a table
Tables are created using the `CREATE TABLE <name_of_table>` directive followed by a pair of parenthesis inside
which is a definition of each of the columns, meaning column names, their respective data type, and if they 
allow `NULL` values or not, default is to allow `NULL` values, and if they are a `PRIMARY KEY`. In addition, create statements can link a table column to another
column using the `REFERENCES` keyword.

### A simple table
The following SQL command creates a students table with six columns 

```SQL
CREATE TABLE Students (
    last_name TEXT, 
    first_name TEXT, 
    username TEXT, 
    exam1 REAL, 
    exam2 REAL, 
    exam3 REAL
);
```

### Inserting some values into a table
You can insert values into a table using the `INSERT INTO <name_of_table> (<name_of_column_1>, <name_of_column_2>, ....)
VALUES (<value_of_column_1>, <value_of_column_1>, ...);` The following SQL statements insert some values into a table with some
variations.


#### Valid Insert statements
```SQL
INSERT INTO Students (last_name, first_name, username, exam1, exam2, exam3) VALUES ('Doe', 'John', 'johndoe', 98, 76, 89);
INSERT INTO Students (first_name, last_name, username, exam1, exam2, exam3) VALUES ('Emily', 'Shepard', 'eshepard', 88, 96, 90);
INSERT INTO Students VALUES ('Doe', 'Jane', 'janedoe', 89, 64, 39);
INSERT INTO Students (last_name, first_name, username) VALUES ('Smith', 'John', 'johnsmith');
INSERT INTO Students (exam1, exam2, exam3) VALUES (78, 99, 83);
INSERT INTO Students (exam1, exam2, exam3) VALUES ('25', '33', '45');
INSERT INTO Students  VALUES (56, 32, 33, 'Eaglestone', 'Ken', 'keagle');
```
1. In the first insert statement all columns names and their corresponding values are supplied in the order of create table statement. 
2. In the second insert statement, the order of the columns names are changed (first and last name), but SQLite3 will put them in the correct column. 
3. In the third insert statement, if all the number of values supplied correspond to the number of rows, then SQLite3 will insert them into the table in the order of the create statement. 
4. In the fourth insert statement, it is possible to only supply some values since by default all columns allow null values. 
5. In the fifth insert statement, only the names exams values a provided. This is allowed because by default all columns allow null values, but his is poor database design. 
6. In the sixth insert statement, only exams values are provided, but they are type string. SQLite3 will convert them to `REAL` before inserting them into the table. 
7. In the seventh insert statement, all the values are inserted into the wrong column, which is allowed by SQLite3 since it is dynamically types, so be very careful!


#### Invalid Insert Statement

```SQL
INSERT INTO Students  VALUES ('Smith', 'John', 'johnsmith');
```

```SQL
Execution finished with errors.
Result: table Students has 6 columns but 3 values were supplied
At line 1:
INSERT INTO Students  VALUES ('Smith', 'Jake', 'johnsmith');
```

1. If fewer values are provided without explicitly mentioning the column names, SQLite3 will raise an error. 


### Adding `NOT NULL` constraint
In the previous table we highlighted a poor database design where you could insert exams scores without a student. 
We fix this by making the student fields `NOT NULL`, meaning they have to be specified. 

Note you can drop a table using `DROP TABLE <name_of_table>`. 

```SQL
DROP TABLE Students;
CREATE TABLE Students (
    last_name TEXT NOT NULL, 
    first_name TEXT NOT NULL, 
    username TEXT NOT NULL, 
    exam1 REAL, 
    exam2 REAL, 
    exam3 REAL
);
```

- Note that the two separate SQL statements are separated by `;`. 

If we know attempt to enter a student exam score without the name fields, SQLite3 will raise an error. 

```SQL
INSERT INTO Students('exam1') VALUES ('52');
```

```SQL
Execution finished with errors.
Result: NOT NULL constraint failed: Students.last_name
At line 10:
INSERT INTO Students('exam1') VALUES ('52');
```