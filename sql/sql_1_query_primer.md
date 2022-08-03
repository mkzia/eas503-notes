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


# SQL Query Primer

## Query Clauses
SQL offers six clauses ({ref}`query-clauses`) to query your data. All SQL queries will use at least two clauses, i.e., `SELECT` and `FROM`. 

```{table} Query Clauses
:name: query-clauses
| **Clause Name** 	| **Purpose**                                                                           	|
|-----------------	|---------------------------------------------------------------------------------------	|
| SELECT          	| Determines which columns to include in the query's result set                         	|
| FROM            	| Identifies the tables from which to retrieve data and how the tables should be joined 	|
| WHERE           	| Filters out unwanted data                                                             	|
| GROUP BY        	| Used to group rows together by common column values                                   	|
| HAVING          	| Filters out unwanted groups                                                           	|
| ORDER BY        	| Sorts the rows of the final result set by one or more columns                         	|
```

(sql:primer:basic)=
### Basic query
The most basic SQL query will have a `SELECT` and `FROM` clause. Select lets you choose the columns
you want. In case you want all the columns, you can use `*`, which indicates to SQL you want all the columns. 
The `FROM` clause lets you specify the table you want to query. The following is the most basic SQL query:
You can select all the columns by using `*` after the `SELECT`. Note all SQL queries are terminated 
by a semicolon (`;`). You can format your SQL with as many spaces and tabs as you like. To indicate to SQL
that your query statement is complete, terminate it with a semicolon. 

Query: Select all columns and all rows from the `PatientCorePopulatedTable` table. 

```sql
SELECT
    *
FROM
    PatientCorePopulatedTable;
```

By default all the rows are selected since no filtering is applied. 

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    *
FROM
    PatientCorePopulatedTable;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:limit_rows)=
### Limit rows
You can limit the number of rows to 10 by adding `LIMIT 10` after the `FROM` clause. 

**Query:** Select all columns and from the `PatientCorePopulatedTable` table and limit to 10 rows. 

```sql
SELECT
    *
FROM
    PatientCorePopulatedTable
LIMIT 10;
```

```{code-cell} ipython3
:tags: ["hide-input"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    *
FROM
    PatientCorePopulatedTable
LIMIT 10;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:select_some_columns)=
### Select some columns
You can select columns from a table by specifying them after the `SELECT` clause. Multiple
columns are separated by a comma (`,`). 

**Query**: Select the PatientID and PatientDateOfBirth columns and limit to 10 rows.

```sql
SELECT
    PatientID,
    PatientDateOfBirth
FROM
    PatientCorePopulatedTable
LIMIT 10;
```

```{code-cell} ipython3
:tags: ["hide-input"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    PatientID,
    PatientDateOfBirth
FROM
    PatientCorePopulatedTable
LIMIT 10;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:using_column_alias)=
### Using column alias
Notice that all the column names are prefixed by `Patient`. You can give columns a different name, meaning an alias. 
There are two ways to do alias. You can put the alias name right after the actual column name, e.g., `PatientID PTID`
or you can use the `AS` keyword to indicate explicitly that you are aliasing a column name, e.g., `PatientID AS PTID`.
Note that if the alias has a space, then it should be in quotes. 

**Query**: Select the PatientID and PatientDateOfBirth columns, but alias PatientID to PTID and PatientDateOfBirth 
to "Date of Birth" and limit to 10 rows.

```sql
SELECT
    PatientID PTID, 
    PatientDateOfBirth AS "Date of Birth"
FROM
    PatientCorePopulatedTable
LIMIT 10;
```

```{code-cell} ipython3
:tags: ["hide-input"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    PatientID PTID, 
    PatientDateOfBirth AS "Date of Birth"
FROM
    PatientCorePopulatedTable
LIMIT 10;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```
(sql:primer:adding_column)=
### Adding columns not from the table
Besides selecting columns in the table, you can also add the following columns to your query:
1. literals such as numbers or strings
1. Math expressions such as `PatientPopulationPercentageBelowPoverty + 1`, or `PatientPopulationPercentageBelowPoverty * 100`

**Query**: Select the PatientID, Hospital, and PatientPopulationPercentageBelowPoverty columns, alias PatientID to PTID, 
make the `Hospital` column 'Buffalo General', multiply `PatientPopulationPercentageBelowPoverty` by 10 and alias it to
`Poverty Level`, and limit to 10 rows. Note that if the alias name has a space, then the name needs to inside of quotes. 

```sql
SELECT
    PatientID PTID, 
    'Buffalo General' Hospital,
    PatientPopulationPercentageBelowPoverty * 10 "Poverty Level"
FROM
    PatientCorePopulatedTable
LIMIT 10;
```

```{code-cell} ipython3
:tags: ["hide-input"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    PatientID PTID, 
    'Buffalo General' Hospital,
    PatientPopulationPercentageBelowPoverty * 10 "Poverty Level"
FROM
    PatientCorePopulatedTable
LIMIT 10;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:removing_duplicates)=
### Removing duplicates
In some cases you might get duplicate rows. You remove these duplicate rows by putting the `DISTINCT` keyword
after the `SELECT` keyword. One use of this is to get distinct values of a given column. 

Query: Select only the `PatientMaritalStatus` column from the `PatientCorePopulatedTable` table.

```sql
SELECT
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

**Query:** Select the `PatientMaritalStatus` column from the `PatientCorePopulatedTable` table but
only select distinct values. 

```sql
SELECT DISTINCT
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT DISTINCT
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:removing_duplicates_2)=
### Removing duplicates with multiple columns
The `DISTINCT` keyword can also be used to find distinct combination of columns. **It is also used sometimes with joins
to remove duplicate rows.**

**Query:** Select  `PatientRace` and `PatientMaritalStatus` columns from the `PatientCorePopulatedTable` table but
only select distinct values. This query finds the distinct combinations of race and martial status. 

```sql
SELECT DISTINCT
    PatientRace,
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable
ORDER BY PatientRace, PatientMaritalStatus
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT DISTINCT
    PatientRace,
    PatientMaritalStatus
FROM
    PatientCorePopulatedTable
ORDER BY PatientRace, PatientMaritalStatus
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### `FROM` multiple tables
The power of SQL lies in the fact that you can combine tables together based on some shared column between
tables. The `FROM` clause allows you to select from multiple tables. 

You should now that there are four types of tables in SQL:
1. Permanent tables (i.e., created using the `CREATE TABLE` statement)
2. Derived tables (i.e., rows returned by a subquery and held in memory)
3. Temporary tables (i.e., volatile data held in memory)
4. Virtual tables (i.e, created using the `CREATE VIEW` statement)

We have been using permanent tables so far. Temporary and virtual tables will be covered later. The following
is an example a derived table. 

(sql:primer:derived_table)=
#### Derived table
A derived query is a query held in memory. You surround it a pair of parenthesis and give it a name. 

**Query**: Create a subquery called `dx_codes` which selects the `PrimaryDiagnosisCode` and `PrimaryDiagnosisDescription`
columns from the `AdmissionsDiagnosesCorePopulatedTable` where the `AdmissionID` is equal to `1`. Then use this derived query in another query that concatenates the diagnosis code and diagnosis description, e.g., "(M01.X) Direct infection of joint in infectious and parasitic diseases classified elsewhere". Fields and string literals can be concatenated in SQLite using `||`. 

```sql
SELECT '(' || dx_codes.code || ') ' || dx_codes.description AS CodeWDescription
FROM
   (
     SELECT
       PrimaryDiagnosisCode code,
       PrimaryDiagnosisDescription description 
     FROM
       AdmissionsDiagnosesCorePopulatedTable
      WHERE AdmissionID = 1
   ) dx_codes
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT '(' || dx_codes.code || ') ' || dx_codes.description AS CodeWDescription
FROM
   (
     SELECT
       PrimaryDiagnosisCode code,
       PrimaryDiagnosisDescription description 
     FROM
       AdmissionsDiagnosesCorePopulatedTable
      WHERE AdmissionID = 1
   ) dx_codes
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:where_clause)=
### The `WHERE` Clause
The `WHERE` clause allows you to filter out unwanted rows. For string fields, you can use the equality operator (`=`) or 
the `LIKE` operator. For numerical and date fields, you can use all the usually operators such as greater than, less than, etc. 
`WHERE` clauses can be combined using `AND` and `OR`. Parenthesis can be used to clarify grouping of the clauses. The
`WHERE` clauses are put after the `FROM` clause. 

**Query:** Select all patients from `PatientCorePopulatedTable` table that are either married and african american or married and white and 
the `PatientPopulationPercentageBelowPoverty` is above 15. Select the following columns: PatientID, PatientRace, PatientMaritalStatus and
PatientPopulationPercentageBelowPoverty. 

```sql
SELECT 
  PatientID, 
  PatientRace, 
  PatientMaritalStatus,
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
WHERE ((PatientRace = 'White' AND PatientMaritalStatus	= 'Married') OR (PatientRace = 'African American' AND PatientMaritalStatus = 'Married'))
  AND PatientPopulationPercentageBelowPoverty > 15
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientID, 
  PatientRace, 
  PatientMaritalStatus,
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
WHERE (
	(
		PatientRace = 'White' AND PatientMaritalStatus	= 'Married') 
		OR (PatientRace = 'African American' AND PatientMaritalStatus	= 'Married')
	)
  AND PatientPopulationPercentageBelowPoverty > 15
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

(sql:primer:order_by_clause)=
### The `ORDER BY` Clauses 
You can order the the rows by column(s) using the `ORDER BY` clause. This clause is put after the `WHERE` clause. You can specify
multiple columns separated by comma. You can also specify ascending order using the `ASC` keyword after the column name 
and descending order by using the `DESC` keyword. The default sorting order is ascending. A shortcut for descending is putting `-` before the column name. Finally
you can sort the columns by its numerical position. 

#### Sort by columns

**Query:** Select the `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` columns from 
the `PatientCorePopulatedTable` table and sort by `PatientPopulationPercentageBelowPoverty`.

```sql
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientPopulationPercentageBelowPoverty;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientPopulationPercentageBelowPoverty;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

**Query:** Select the `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` columns from 
the `PatientCorePopulatedTable` table and sort by `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty`.

```sql
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, PatientPopulationPercentageBelowPoverty;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, PatientPopulationPercentageBelowPoverty;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


#### Ascending versus Descending Sort Order

**Query:** Select the `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` columns from 
the `PatientCorePopulatedTable` table and sort by `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` by descending order
using the keyword `DESC`.

```sql
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, PatientPopulationPercentageBelowPoverty DESC;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, PatientPopulationPercentageBelowPoverty DESC;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

**Query:** Select the `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` columns from 
the `PatientCorePopulatedTable` table and sort by `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` by descending order
using the `-`.

```sql
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, -PatientPopulationPercentageBelowPoverty;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, -PatientPopulationPercentageBelowPoverty;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


#### Sorting using numerical position
When sorting by numerical position, you cannot use `-`. You must use the keyword `DESC`. Column names make your code more explicit, which you should prefer. 

**Query:** Select the `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` columns from 
the `PatientCorePopulatedTable` table and sort by `PatientMaritalStatus` and `PatientPopulationPercentageBelowPoverty` by descending order
using the keyword `DESC`, but using the numerical position of `PatientPopulationPercentageBelowPoverty`. 

```sql
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, 2 DESC;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
  PatientMaritalStatus, 
  PatientPopulationPercentageBelowPoverty
FROM
  PatientCorePopulatedTable
ORDER BY PatientMaritalStatus, 2 DESC;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')

