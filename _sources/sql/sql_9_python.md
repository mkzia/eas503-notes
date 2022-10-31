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

# Python and SQLite3

## Basic example
- This is a basic example to show you how to use Pandas to open a database file and execute an SQL
query. 

```python
import pandas as pd
import sqlite3
conn = sqlite3.connect("join_example_database.db")
sql_statement = "select Quantities.product, Quantities.quantity FROM Quantities"
df = pd.read_sql_query(sql_statement, conn)
display(df)
```
 

## Homework Example
- Homework will ask you to write an SQL query. In that case you only have to update the `sql_statement` variable and leave rest of the code as is. 


```python
def dbex_1():
    """
    Return all products with quantity greater than 60;
    output columns: product_name, product_quantity
    output (df, sql_statement)
    """

    import pandas as pd
    import sqlite3
    conn = sqlite3.connect("join_example_database.db")
    cur = conn.cursor()
    sql_statement = "select Quantities.product, Quantities.quantity FROM Quantities Where quantity > 60;"
    df = pd.read_sql_query(sql_statement, conn)
    return (df, sql_statement)
```

- The following code shows how the `sql_statement` will be verified. 

```python
df, sql_statement = dbex_1()

data = pd.read_csv("dbex_1.csv") 
conn = sqlite3.connect("join_example_database.db")
cur = conn.cursor()
df = pd.read_sql_query(sql_statement, conn)

assert df.equals(data) == True
```

## Utility functions
- Two utility functions are provided that you will use often

```python
import sqlite3
from sqlite3 import Error


def create_connection(db_file):
    conn = None
    try:
        conn = sqlite3.connect(db_file)
    except Error as e:
        print(e)

    return conn


def create_table(conn, create_table_sql):
    try:
        c = conn.cursor()
        c.execute(create_table_sql)
    except Error as e:
        print(e)
```
- `create_connection` opens an sqlite3 file for writing and `create_table` function creates a table. 

## Example SQL query using Python

```python
db_file = 'student_test_in_class.db'
conn = create_connection(db_file)
cur = conn.cursor()
cur.execute("SELECT first_name, last_name FROM students")

rows = cur.fetchall()

for row in rows:
    print(row)
```

## Steps to creating a database using Python
1. Write a create table sql statement(s)
2. Write insert function(s)
3. Read files or data and use the insert function(s) to insert data into table

### Create a database and populate from a file
```python
import os
db_file = 'student_test_in_class.db'
if os.path.exists(db_file):
    os.remove(db_file)

create_table_sql = """
CREATE TABLE students (
    last_name TEXT,
    first_name TEXT,
    username TEXT,
    exam1 REAL,
    exam2 REAL,
    exam3 REAL
);
"""

conn = create_connection(db_file)


def insert_student(conn, values):
    sql = ''' INSERT INTO students(last_name,first_name,username,exam1,exam2,exam3)
              VALUES(?,?,?,?,?,?) '''
    cur = conn.cursor()
    cur.execute(sql, values)
    return cur.lastrowid

with conn:
    # create
    create_table(conn, create_table_sql)

    # insert
    for student in open('students.tsv', 'r'):
        values = student.strip().split('\t')
        print(values)
        rid = insert_student(conn, values) ## What is rid?

```

### Fetch data

```python
sql_statement = "SELECT * FROM Students"
df = pd.read_sql_query(sql_statement, conn)
display(df)
```

## Another example
- You must understand that the insert statements take in a tuple even if you are inserting one value!!!!

```python
# 1 -- create table sql statements
# 2 -- create insert functions
# 3 -- read files or data and use the insert function


db = 'depts_students.db'
if os.path.exists(db):
    os.remove(db)

create_table_departments_sql = """ CREATE TABLE [Departments] (
    [DepartmentId] INTEGER  NOT NULL PRIMARY KEY,
    [DepartmentName] TEXT 
); """

create_table_students_sql = """CREATE TABLE [Students] (
    [StudentId] INTEGER  PRIMARY KEY NOT NULL,
    [StudentName] TEXT NOT NULL,
    [DepartmentId] INTEGER,
    [DateOfBirth] DATE,
    FOREIGN KEY(DepartmentId) REFERENCES Departments(DepartmentId)
);"""


depts = ('IT', 'Physics', 'Arts', 'Math')

students = (
    ('Michael', 1, '1998-10-12'),
    ('John', 1, '1998-10-12'),
    ('Jack', 1, '1998-10-12'),
    ('Sara', 2, '1998-10-12'),
    ('Sally', 2, '1998-10-12'),
    ('Jena', None, '1998-10-12'),
    ('Nancy', 2, '1998-10-12'),
    ('Adam', 3, '1998-10-12'),
    ('Stevens', 3, '1998-10-12'),
    ('George', None, '1998-10-12')
)

def insert_depts(conn, values):
    sql = ''' INSERT INTO Departments(DepartmentName)
              VALUES(?) '''
    cur = conn.cursor()
    cur.execute(sql, values)
    return cur.lastrowid


def insert_student(conn, values):
    sql = ''' INSERT INTO Students(StudentName, DepartmentId, DateOfBirth)
              VALUES(?,?,?) '''
    cur = conn.cursor()
    cur.execute(sql, values)
    return cur.lastrowid

conn = create_connection(db)

with conn:

    create_table(conn, create_table_departments_sql)
    create_table(conn, create_table_students_sql)
    for values in depts:
        insert_depts(conn, (values, ))
        
    for values in students:
        insert_student(conn, values)
```

### Turning a single a value into tuple
```
depts = ('IT', 'Physics', 'Arts', 'Math')

for val in depts:
    print(val)
    print((val,))
```


### Table 
```
sql_statement = "SELECT * FROM Students"
df = pd.read_sql_query(sql_statement, conn)
display(df)
```

### Fetch all
```python
cur = conn.cursor()
cur.execute('SELECT * FROM Departments')
for row in cur.fetchall():  
    print(row)
```

### Fetch all again
- Why no data?
```python
for row in cur.fetchall():
    print(row)
```

### Fetch one

```python
cur.execute('SELECT * FROM Departments')
print(cur.fetchone())
print(cur.fetchone())
print(cur.fetchone())
print(cur.fetchone())
print(cur.fetchone())
```

## Creating a foreign key dictionary 
- Creating a foreign key dictionary lookup speeds things up because you are avoiding multiple call to the database

```python
conn = create_connection(db)
cur = conn.cursor()
cur.execute('SELECT * FROM Departments')
dept_fk_lookup = {}
for row in cur.fetchall():
    key, text = row
    dept_fk_lookup[text] = key
print(dept_fk_lookup)
```

## Insert data using FK lookup dictionary

```python
db = 'depts_students.db'
if os.path.exists(db):
    os.remove(db)

create_table_departments_sql = """ CREATE TABLE [Departments] (
    [DepartmentId] INTEGER  NOT NULL PRIMARY KEY,
    [DepartmentName] TEXT 
); """

create_table_students_sql = """CREATE TABLE [Students] (
    [StudentId] INTEGER  PRIMARY KEY NOT NULL,
    [StudentName] TEXT NOT NULL,
    [DepartmentId] INTEGER,
    [DateOfBirth] DATE,
    FOREIGN KEY(DepartmentId) REFERENCES Departments(DepartmentId)
);"""

depts = ('IT', 'Physics', 'Arts', 'Math')
students = (
    ('Michael', 'IT', '1998-10-12'),
    ('John', 'IT', '1998-10-12'),
    ('Jack', 'IT', '1998-10-12'),
    ('Sara', 'Physics', '1998-10-12'),
    ('Sally', 'Physics', '1998-10-12'),
    ('Jena', None, '1998-10-12'),
    ('Nancy', 'Physics', '1998-10-12'),
    ('Adam', 'Arts', '1998-10-12'),
    ('Stevens', 'Arts', '1998-10-12'),
    ('George', None, '1998-10-12')
)


with conn:

    create_table(conn, create_table_departments_sql)
    create_table(conn, create_table_students_sql)
    for values in depts:
        insert_depts(conn, (values, ))
        
    for values in students:
        values = list(values)
        print('BEFORE', values)
        key = values[1]
        print('key', key)
        if key:
            values[1] = dept_fk_lookup[values[1]]
        print('AFTER',values)
        insert_student(con
```