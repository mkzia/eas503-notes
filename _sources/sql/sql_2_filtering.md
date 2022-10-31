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

# Filtering

## Logical Operators

| Operator | What it does?                                        |
|----------|------------------------------------------------------|
| `AND`    | True if both conditions are true  | 
| `OR`     | True if one of two conditions is true |
| `NOT`    | Negate a specified condition |
| `IN` | Allows for multiple OR conditions |
| `NOT IN`    | Negate multiple AND conditions  |
| `EXISTS`    | True if a record exists |
| `LIKE`    | True if there is a string match using % |


## Relational Operators
Assume `a=1` and `b=1`

| Relational Operators | What it does?             |
|----|---------------------------------------------|
| = | True if a has the same value as b           |
| <>, != | True if a does not have the same value as b |
| >  | True if a is greater than b                 |
| <  | True if a is less than b                    |
| >= | True if a is greater than or equal to b     |
| <= | True if a is less than or equal to b        |


## Conditional Evaluation 

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientGender = 'Male' AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientGender = 'Male' AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Using Parenthesis

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE (PatientGender = 'Male' OR PatientRace = 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE (PatientGender = 'Male' OR PatientRace = 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Using the `NOT` Operator

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE NOT (PatientGender = 'Male' OR PatientRace = 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE NOT (PatientGender = 'Male' OR PatientRace = 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Inequality condition

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE (PatientGender = 'Male' AND PatientRace <> 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE (PatientGender = 'Male' AND PatientRace <> 'White') AND PatientDateOfBirth < '1950-01-01'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Range using `BETWEEN` condition

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientDateOfBirth BETWEEN '1920-01-01' AND '1965-01-01'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientDateOfBirth BETWEEN '1920-01-01' AND '1965-01-01'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## String Condition

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientID BETWEEN '2A' AND '53'
ORDER BY PatientID
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientID BETWEEN '2A' AND '53'
ORDER BY PatientID
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Membership Condition

```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace = 'White' OR PatientRace = 'Asian'
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace = 'White' OR PatientRace = 'Asian'
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


### `IN` condition
```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace IN ('White', 'Asian')
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace IN ('White', 'Asian')
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### `NOT IN` condition
```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace NOT IN ('White', 'Asian')
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientRace NOT IN ('White', 'Asian')
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

### Using subqueries
```SQL
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientID IN (SELECT PatientID FROM PatientCorePopulatedTable WHERE PatientDateOfBirth > '1980-01-01')
ORDER BY PatientDateOfBirth
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM PatientCorePopulatedTable
WHERE PatientID IN (SELECT PatientID FROM PatientCorePopulatedTable WHERE PatientDateOfBirth > '1980-01-01')
ORDER BY PatientDateOfBirth
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Using Wildcards

```SQL
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE 'M%'
ORDER BY PrimaryDiagnosisCode
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE 'M%'
ORDER BY PrimaryDiagnosisCode
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE '%4'
ORDER BY PrimaryDiagnosisCode
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE '%4'
ORDER BY PrimaryDiagnosisCode
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE '%5.%'
ORDER BY PrimaryDiagnosisCode
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM AdmissionsDiagnosesCorePopulatedTable
WHERE PrimaryDiagnosisCode LIKE '%5.%'
ORDER BY PrimaryDiagnosisCode
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Checking for NULL

```SQL
SELECT *
FROM Cars
WHERE color_id IS NULL
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('cars.db')

sql_statement = """
SELECT *
FROM Cars
WHERE color_id IS NULL
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```