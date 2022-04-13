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

# Grouping and Aggregates

The `GROUP BY` allows you to group values together and then apply one of the following five functions
1. `MAX()`
2. `MIN()`
3. `AVG()`
4. `SUM()`
5. `COUNT()`

The `HAVING` keyword, which follows the `GROUP BY` keyword allows you to filter the aggregated results. 

## A Simple Grouping Examples

Find the total number of lab results for each patients

```SQL
SELECT PatientID, Count(LabName) LabCount
FROM LabsCorePopulatedTable 
GROUP BY PatientID
ORDER BY -LabCount
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT PatientID, Count(LabName) LabCount
FROM LabsCorePopulatedTable 
GROUP BY PatientID
ORDER BY -LabCount
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT PatientID, Count(LabName) LabCount
FROM LabsCorePopulatedTable 
GROUP BY PatientID
HAVING LabCount > 2000
ORDER BY -LabCount
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT PatientID, Count(LabName) LabCount
FROM LabsCorePopulatedTable 
GROUP BY PatientID
HAVING LabCount > 2000
ORDER BY -LabCount
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT PatientID, Count(LabName) LabCount, Max(LabValue),  Min(LabValue),  SUM(LabValue), ROUND(AVG(LabValue),2) AVERAGE
FROM LabsCorePopulatedTable 
WHERE LabName = 'URINALYSIS: RED BLOOD CELLS'
GROUP BY PatientID
ORDER BY -AVERAGE
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT PatientID, Count(LabName) LabCount, Max(LabValue),  Min(LabValue),  SUM(LabValue), ROUND(AVG(LabValue),2) AVERAGE
FROM LabsCorePopulatedTable 
WHERE LabName = 'URINALYSIS: RED BLOOD CELLS'
GROUP BY PatientID
ORDER BY -AVERAGE
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Multicolumn Grouping

```SQL
SELECT PatientID, LabName, Count(LabName) LabCount, Max(LabValue),  Min(LabValue),  SUM(LabValue), ROUND(AVG(LabValue),2) AVERAGE
FROM LabsCorePopulatedTable 
GROUP BY PatientID, LabName
ORDER BY -PatientID, -LabCount
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT PatientID, LabName, Count(LabName) LabCount, Max(LabValue),  Min(LabValue),  SUM(LabValue), ROUND(AVG(LabValue),2) AVERAGE
FROM LabsCorePopulatedTable 
GROUP BY PatientID, LabName
ORDER BY -PatientID, -LabCount
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```



## Date Difference

```SQL
SELECT PatientID, ROUND(MAX(julianday(AdmissionEndDate)-julianday(AdmissionStartDate))) MAX_STAY
FROM AdmissionsCorePopulatedTable 
GROUP BY PatientID
HAVING MAX_STAY >= 20
ORDER BY -MAX_STAY
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT PatientID, ROUND(MAX(julianday(AdmissionEndDate)-julianday(AdmissionStartDate))) MAX_STAY
FROM AdmissionsCorePopulatedTable 
GROUP BY PatientID
HAVING MAX_STAY >= 20
ORDER BY -MAX_STAY
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```