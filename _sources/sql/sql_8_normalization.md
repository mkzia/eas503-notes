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
# Normalization

What is database normalization?
- Ref: https://www.complexsql.com/database-normalization/
- Ref: http://www.databasedev.co.uk/1norm_form.html
- The purpose of database normalization is to:
- eliminate redundant data
- reduce complexity of data, making it easier to manage the data and make change
- ensure logical data dependencies
- How is database normalization achieved?
  - By fulfilling five normal forms. Each normal form represents an increasingly stringent set of rules. Usually fulfilling the first three normal forms is sufficient.
  - Ref: https://www.1keydata.com/database-normalization/first-normal-form-1nf.php
- First Normal Form  (1NF): 
  1. if there are no repeating groups.
  2. all values are atomic, meaning they are the smallest meaningful value
- Second Normal Form  (2NF): 
  1. the table is in first normal form
  2. each non-key field is functionally dependent on the entire primary key
- Third Normal Form (3NF):
  1. the table is in second normal form
  2. there are no transitive dependencies
- Ref: https://arctype.com/blog/2nf-3nf-normalization-example/
- Summary
  1. All values must be atomic
  2. No redundancy
  3. No implicit relationship/dependency
  4. No transitive relationship/dependency

## Bad Design Examples
### Example 1

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example1.db')

sql_statement = "SELECT * FROM EMPLOYEES_PROJECTS_TIME"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Problems with example1
  - Repeating group of fields
  - The project and time fields are not made up of atomic values
  - Can't sort by last name
  - Can't sort by time because field is type text
  - Assumed relationship between project and time

### Example 2

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example2.db')

sql_statement = "SELECT * FROM EMPLOYEES_PROJECTS_TIME"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Analysis of example2
  - Can sort now!
  - How can you add another project?

### Example 3

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example3.db')

sql_statement = "SELECT * FROM EMPLOYEES"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

sql_statement = "SELECT * FROM PROJECTS_EMPLOYEES_TIME"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Analysis of example3 -- first normal form
  - Can do groups by employeeid or projectnum
  - Can sort by time
  - Can sort by name


### Example 4

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example4.db')

sql_statement = "SELECT * FROM EMPLOYEES_PROJECTS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Analysis of example4
  - How would you update the project title for a given project? Have to edit in many places
  - Can you add a project without an employeeid?
  - How can you delete a project?

### Example 5

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example5.db')

sql_statement = "SELECT * FROM EMPLOYEES"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

sql_statement = "SELECT * FROM EMPLOYEES_PROJECTS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

sql_statement = "SELECT * FROM PROJECTS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Analysis of example5
  - second normal form

### Example 6

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example6.db')
sql_statement = "SELECT * FROM PROJECTS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

- Analysis of example 6
  - Phone number, which is a non-key field, has transitive dependency on another non-key field. 

### Example 6

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('example7.db')
sql_statement = "SELECT * FROM MANAGERS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
display(df)

conn = sqlite3.connect('example7.db')
sql_statement = "SELECT * FROM PROJECTS"
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')

```

- Analysis of example7
  - Removed transitive dependency 



