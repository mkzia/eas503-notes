---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

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

## Primary Keys
When you create any table, SQLite3 adds a hidden column called `rowid`. 

```SQL
DROP TABLE IF EXISTS Teachers;
CREATE TABLE Teachers(
  TeacherName  TEXT NOT NULL
);

INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
SELECT rowid, * FROM Teachers;
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('primary_key_example.db')

sql_statements = """
DROP TABLE IF EXISTS Teachers;
CREATE TABLE Teachers(
  TeacherName  TEXT NOT NULL
);

INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
SELECT rowid, * FROM Teachers;
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)


sql_statement = """
SELECT * FROM Teachers;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

sql_statement = """
SELECT rowid, * FROM Teachers;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```

1. Note: `DROP TABLE IF EXISTS Teachers;` is used to drop the table if it exists. This is useful if you want to run the create statement over and over again with changes. 


When you create a table that has an INTEGER NOT NULL PRIMARY KEY column, this column is the alias of the rowid column.

```{note}
It uniquely identifies a row in a table. Primary keys have the following properties:
1. Primary keys can consist of one or more columns, meaning multiple columns combined can uniquely define a row. 
2. Primary keys cannot be duplicated since they uniquely identify a row in a table. 
3. Primary keys cannot be NULL since a blank value cannot identify a row in a table. 
4. There can only be one primary key per table. Note that this is different from a primary key being made up of multiple tables. 
5. Primary keys are indexed automatically. 
6. INTEGER primary keys are auto-increment fields starting at 1. 
```

```SQL
DROP TABLE Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL
);
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
SELECT * FROM Teachers;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('primary_key_example.db')

sql_statements = """
DROP TABLE Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL
);
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
INSERT INTO Teachers ('TeacherName') VALUES ('John Smith');
SELECT * FROM Teachers;
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)


sql_statement = """
SELECT * FROM Teachers;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Adding a unique constraint
In the previous examples teachers table, it is possible to add multiple John Smiths. How can you identify one John Smith
from another John Smith. One way would be to assign teachers unique IDs that cannot be reused. A column can be made unique
by using the `UNIQUE` keyword. 

```
DROP TABLE Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL,
   TeacherEmployeeID INTEGER NOT NULL,
   UNIQUE (TeacherEmployeeID)
);
INSERT INTO Teachers ('TeacherName', 'TeacherEmployeeID') VALUES ('John Smith', 100001);
INSERT INTO Teachers ('TeacherName', 'TeacherEmployeeID') VALUES ('John Smith', 100002);
SELECT * FROM Teachers;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('primary_key_example.db')

sql_statements = """
DROP TABLE Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL,
   TeacherEmployeeID INTEGER NOT NULL,
   UNIQUE (TeacherEmployeeID)
);
INSERT INTO Teachers ('TeacherName', 'TeacherEmployeeID') VALUES ('John Smith', 100001);
INSERT INTO Teachers ('TeacherName', 'TeacherEmployeeID') VALUES ('John Smith', 100002);
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)

sql_statement = """
SELECT * FROM Teachers;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## Foreign Keys
What is a foreign key? In a relational database, you can relate one table to another table. 
The two tables can be related if and only if both tables have one column in common. 
This column has to be declared as a INTEGER/TEXT data type that cannot be NULL and has the PRIMARY KEY constraint, e.g., 
`<name_of_column> INTEGER NOT NULL PRIMARY KEY`.It seems INTEGER foreign keys are preferred over TEXT ones. See the following
references:
   - https://stackoverflow.com/questions/3162202/sql-primary-key-integer-vs-varchar
   - https://stackoverflow.com/questions/9206391/int-vs-varchar-datatype-for-primary-keys


```{warning}
Foreign key constraint are NOT enabled by default in SQLite. Execute `PRAGMA foreign_keys = ON;` to enable foreign
key constraint. 
```

### Creating a relationship

```SQL
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS  Courses;
DROP TABLE IF EXISTS Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL,
   TeacherEmployeeID INTEGER NOT NULL,
   UNIQUE (TeacherEmployeeID)
);

CREATE TABLE Courses(
  CourseId     INTEGER NOT NULL PRIMARY KEY, 
  CourseName   TEXT NOT NULL,
  CourseShortID   TEXT NOT NULL,
  TeacherId INTEGER NOT NULL,
  FOREIGN KEY(TeacherId) REFERENCES Teachers(TeacherId),
  UNIQUE (CourseName, CourseShortID)
);

INSERT INTO Teachers (TeacherName, TeacherEmployeeID)
VALUES ('Melissa Larson', 10001);

INSERT INTO Teachers (TeacherName,TeacherEmployeeID)
VALUES ('Christopher Smith', 10002);

INSERT INTO Courses (CourseName, CourseShortID, TeacherId)
VALUES ('Introduction to Python', 'EAS503', 1);

INSERT INTO Courses (CourseName, CourseShortID, TeacherId)
VALUES ('Introduction to Probability', 'EAS506', 2);
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('teachers_courses.db')

sql_statements = """
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS  Courses;
DROP TABLE IF EXISTS Teachers;
CREATE TABLE Teachers (
   TeacherId INTEGER NOT NULL PRIMARY KEY,
   TeacherName  TEXT NOT NULL,
   TeacherEmployeeID INTEGER NOT NULL,
   UNIQUE (TeacherEmployeeID)
);

CREATE TABLE Courses(
  CourseId     INTEGER NOT NULL PRIMARY KEY, 
  CourseName   TEXT NOT NULL,
  CourseShortID   TEXT NOT NULL,
  TeacherId INTEGER NOT NULL,
  FOREIGN KEY(TeacherId) REFERENCES Teachers(TeacherId),
  UNIQUE (CourseName, CourseShortID)
);

INSERT INTO Teachers (TeacherName, TeacherEmployeeID)
VALUES ('Melissa Larson', 10001);

INSERT INTO Teachers (TeacherName,TeacherEmployeeID)
VALUES ('Christopher Smith', 10002);

INSERT INTO Courses (CourseName, CourseShortID, TeacherId)
VALUES ('Introduction to Python', 'EAS503', 1);

INSERT INTO Courses (CourseName, CourseShortID, TeacherId)
VALUES ('Introduction to Probability', 'EAS506', 2);
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```


### Using the relationship

Method 1 -- explicitly link two tables
- Note this method will result in duplicated TeacherID fields if you select all the columns using '*'.


```SQL
SELECT 
    *
FROM 
    Teachers t
INNER JOIN 
    Courses c ON t.TeacherId = c.TeacherId;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('teachers_courses.db')

sql_statement = """
SELECT 
    *
FROM 
    Teachers t
INNER JOIN 
    Courses c ON t.TeacherId = c.TeacherId;
"""

df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```


```SQL
SELECT 
    t.TeacherName, 
    c.CourseName, 
    c.CourseShortID
FROM 
    Teachers t
INNER JOIN 
    Courses c ON t.TeacherId = c.TeacherId;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('teachers_courses.db')

sql_statement = """
SELECT 
    t.TeacherName, 
    c.CourseName, 
    c.CourseShortID
FROM 
    Teachers t
INNER JOIN 
    Courses c ON t.TeacherId = c.TeacherId;
"""

df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```


Method 2 -- let SQLite infers join condition -- but you specify the shared column name
- Note this method will NOT result in duplicated TeacherID fields if you select all the columns using '*'.

```SQL
SELECT 
    *
FROM 
    Teachers t
INNER JOIN Courses c USING(TeacherId);
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('teachers_courses.db')

sql_statement = """
SELECT  
    *
FROM 
    Teachers t
INNER JOIN Courses c USING(TeacherId);
"""

df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```

```SQL
SELECT 
    t.TeacherName, 
    c.CourseName, 
    c.CourseShortID
FROM 
    Teachers t
INNER JOIN Courses c USING(TeacherId);
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('teachers_courses.db')

sql_statement = """
SELECT 
    t.TeacherName, 
    c.CourseName, 
    c.CourseShortID
FROM 
    Teachers t
INNER JOIN Courses c USING(TeacherId);
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```


## Joining Tables
There are two types of joins
- Inner Join
- Outer Join
    - Left Outer Join (or Left Join)
    - Right Outer Join (or Right Join)
    - Full Outer Join (or Full Join)
Example database: `join_example_database.db`
    - Ref: https://www.diffen.com/difference/Inner_Join_vs_Outer_Join

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('join_example_database.db')

sql_statement = """
SELECT
    *
FROM Prices 
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

sql_statement = """
SELECT
    *
FROM Quantities 
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)
```

### Inner Join

```SQL
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM Prices 
INNER JOIN Quantities ON Prices.Product = Quantities.Product;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('join_example_database.db')

sql_statement = """
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM Prices 
INNER JOIN Quantities ON Prices.Product = Quantities.Product;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


### Left Join

```SQL
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Prices
LEFT OUTER JOIN Quantities ON Prices.Product = Quantities.Product;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('join_example_database.db')

sql_statement = """
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Prices
LEFT OUTER JOIN Quantities ON Prices.Product = Quantities.Product;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Right Join -- Doesn't work in SQLITE3

```SQL
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Prices 
RIGHT OUTER JOIN Quantities ON Prices.Product = Quantities.Product;
```

```SQL
Execution finished with errors.
Result: RIGHT and FULL OUTER JOINs are not currently supported
At line 1:
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Prices 
RIGHT OUTER JOIN Quantities
```

### Right Join by doing Left Join 
Change the order of the tables to emulate right join!

```SQL
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Quantities 
LEFT OUTER JOIN Prices ON Quantities.Product = Prices.Product;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('join_example_database.db')

sql_statement = """
SELECT 
    Prices.Product, 
    Prices.Price, 
    Quantities.Quantity
FROM 
    Quantities 
LEFT OUTER JOIN Prices ON Quantities.Product = Prices.Product;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Full Outer join -- does not work in SQLite3

```SQL
SELECT Prices.Product, Prices.Price, Quantities.Quantity
FROM Prices FULL OUTER JOIN Quantities
ON Prices.Product = Quantities.Product;
```

```SQL
Execution finished with errors.
Result: RIGHT and FULL OUTER JOINs are not currently supported
At line 1:
SELECT Prices.Product, Prices.Price, Quantities.Quantity
FROM Prices FULL OUTER JOIN Quantities
```

### Emulate Full outer join
You can use unions to emulate full outer join
Ref: https://www.sqlitetutorial.net/sqlite-full-outer-join/


```SQL
SELECT Prices.Product, Prices.Price,  Quantities.Product, Quantities.Quantity
FROM Prices 
LEFT OUTER JOIN Quantities
ON Prices.Product = Quantities.Product
UNION ALL
SELECT Prices.Product, Prices.Price, Quantities.Product, Quantities.Quantity
FROM Quantities 
LEFT OUTER JOIN Prices
ON Quantities.Product = Prices.Product;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('join_example_database.db')

sql_statement = """
SELECT Prices.Product, Prices.Price,  Quantities.Product, Quantities.Quantity
FROM Prices 
LEFT OUTER JOIN Quantities
ON Prices.Product = Quantities.Product
UNION ALL
SELECT Prices.Product, Prices.Price, Quantities.Product, Quantities.Quantity
FROM Quantities 
LEFT OUTER JOIN Prices
ON Quantities.Product = Prices.Product;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## Joining Multiple Tables

```SQL
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;


CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER NOT NULL,
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id),
   FOREIGN KEY(color_id) REFERENCES Colors(color_id)
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Green');

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Ford', 'Explorer', 2019);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camry', 2010);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;

CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER NOT NULL,
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id),
   FOREIGN KEY(color_id) REFERENCES Colors(color_id)
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Green');

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Ford', 'Explorer', 2019);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camry', 2010);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```

```SQL
SELECT * FROM Colors;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT * FROM Colors;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT * FROM MakeModels;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT * FROM MakeModels;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT * FROM Cars;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT * FROM Cars;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Updating Tables
The following tables have typos in them!

```SQL
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;


CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER NOT NULL,
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id),
   FOREIGN KEY(color_id) REFERENCES Colors(color_id)
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Greene'); -- TYPO!!!

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Frd', 'Explorer', 2019); -- TYPO!!!
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camrey', 2010); -- TYPO!!!
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;

CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER NOT NULL,
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id),
   FOREIGN KEY(color_id) REFERENCES Colors(color_id)
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Greene'); -- TYPO!!!

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Frd', 'Explorer', 2019); -- TYPO!!!
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camrey', 2010); -- TYPO!!!
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Update Color

```SQL
UPDATE Colors
SET color = 'Green'
WHERE color = "Greene";
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
UPDATE Colors
SET color = 'Green'
WHERE color = "Greene";
"""
with conn:
    cur = conn.cursor()
    cur.execute(sql_statement)
```


```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Update MakeModel

```SQL
UPDATE MakeModels
SET Make = 'Ford'
WHERE Make = "Frd";

UPDATE MakeModels
SET Model = 'Camry'
WHERE Model = "Camrey";
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
UPDATE MakeModels
SET Make = 'Ford'
WHERE Make = "Frd";

UPDATE MakeModels
SET Model = 'Camry'
WHERE Model = "Camrey";
"""
with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```


```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Update Car

```SQL
UPDATE Cars
SET available = 1
WHERE car_id = 6;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
UPDATE Cars
SET available = 1
WHERE car_id = 6;
"""
with conn:
    cur = conn.cursor()
    cur.execute(sql_statement)
```


```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Simple Delete
You can easily delete a non-referenced row.

```SQL
DELETE FROM Cars
WHERE available = 0;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
DELETE FROM Cars
WHERE available = 0;
"""
with conn:
    cur = conn.cursor()
    cur.execute(sql_statement)
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## Deleting a referenced row
A referenced row can be deleted or not depending on how the table was defined. By default, a referenced
row cannot be deleted. This default behavior can be changed. You can either set the foreign key to
`ON DELETE SET NULL` or `ON DELETE SET CASCADE`. In the former case, the references to the row being deleted 
is set to `NULL`. In the latter case, the anything that references the row being deleted is also deleted. 

```SQL
DELETE FROM Colors
WHERE color = "Red";
```

```SQL
Execution finished with errors.
Result: FOREIGN KEY constraint failed
At line 1:
DELETE FROM Colors
WHERE color = "Red";
```

```SQL
DELETE FROM Cars
WHERE car_id IN (SELECT car_id from Cars INNER JOIN Colors clr USING(color_id ) WHERE clr.color='Red');
DELETE FROM Colors
WHERE color = "Red";
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
DELETE FROM Cars
WHERE car_id IN (SELECT car_id from Cars INNER JOIN Colors clr USING(color_id ) WHERE clr.color='Red');
DELETE FROM Colors
WHERE color = "Red";
"""
with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```

```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### `ON DELETE`

```SQL
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;


CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
   
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER, --> Have to allow for NULL values for ON DELETE SET NULL to work!!!!
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id) ON DELETE CASCADE,
   FOREIGN KEY(color_id) REFERENCES Colors(color_id) ON DELETE SET NULL
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Green');

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Ford', 'Explorer', 2019);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camry', 2010);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
PRAGMA foreign_keys = ON;
DROP TABLE IF EXISTS Cars;
DROP TABLE IF EXISTS MakeModels;
DROP TABLE IF EXISTS Colors;


CREATE TABLE Colors (
   color_id INTEGER NOT NULL PRIMARY KEY,
   color  TEXT NOT NULL,
   UNIQUE (color)
   
);

CREATE TABLE MakeModels (
  make_model_id     INTEGER NOT NULL PRIMARY KEY, 
  Make   TEXT NOT NULL,
  Model   TEXT NOT NULL,
  Year INTEGER NOT NULL,
  UNIQUE (Make, Model, Year)
);

CREATE TABLE Cars (
   car_id INTEGER NOT NULL PRIMARY KEY,
   make_model_id INTEGER NOT NULL,
   color_id INTEGER, --> Have to allow for NULL values for ON DELETE SET NULL to work!!!!
   available INTEGER NOT NULL,
   FOREIGN KEY(make_model_id) REFERENCES MakeModels(make_model_id) ON DELETE CASCADE,
   FOREIGN KEY(color_id) REFERENCES Colors(color_id) ON DELETE SET NULL
);

INSERT INTO Colors (color) VALUES ('Red');
INSERT INTO Colors (color) VALUES ('Blue');
INSERT INTO Colors (color) VALUES ('Green');

INSERT INTO MakeModels (Make, Model, Year) VALUES ('Ford', 'Explorer', 2019);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Toyota', 'Camry', 2010);
INSERT INTO MakeModels (Make, Model, Year) VALUES ('Honda', 'Accord', 2015);

INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (2, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 1, 1);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (1, 2, 0);
INSERT INTO Cars (make_model_id, color_id, available) VALUES (3, 3, 0);
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```


```SQL
DELETE FROM Colors
WHERE color = "Red";

DELETE FROM MakeModels
WHERE make_model_id = 1;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statements = """
DELETE FROM Colors
WHERE color = "Red";

DELETE FROM MakeModels
WHERE make_model_id = 1;
"""

with conn:
    cur = conn.cursor()
    cur.executescript(sql_statements)
```


```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    INNER JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    INNER JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    LEFT JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    LEFT JOIN Colors ON Colors.color_id = Cars.color_id;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT Cars.car_id, MakeModels.Make, MakeModels.Model, MakeModels.year, Colors.color, Cars.available
FROM Cars
    LEFT JOIN MakeModels ON MakeModels.make_model_id = Cars.make_model_id
    LEFT JOIN Colors ON Colors.color_id = Cars.color_id;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```