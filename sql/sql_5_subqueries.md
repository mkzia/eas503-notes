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

# Subqueries

## Using value from a subquery

```SQL
SELECT p.*, m.max_stay
FROM  PatientCorePopulatedTable  p
INNER JOIN 
(SELECT PatientID, ROUND(MAX(julianday(AdmissionEndDate)-julianday(AdmissionStartDate))) MAX_STAY
FROM AdmissionsCorePopulatedTable 
GROUP BY PatientID
HAVING MAX_STAY >= 15
) m
ON p.PatientID = m.PatientID
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT p.*, m.max_stay
FROM  PatientCorePopulatedTable  p
INNER JOIN 
(SELECT PatientID, ROUND(MAX(julianday(AdmissionEndDate)-julianday(AdmissionStartDate))) MAX_STAY
FROM AdmissionsCorePopulatedTable 
GROUP BY PatientID
HAVING MAX_STAY >= 15
) m
ON p.PatientID = m.PatientID
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## `IN` and `NOT IN` examples

```SQL
SELECT *
FROM  LabsCorePopulatedTable 
WHERE PatientID IN (
    SELECT PatientID 
    FROM PatientCorePopulatedTable 
    WHERE PatientLanguage IN ('Icelandic', 'Spanish')
)
LIMIT 100;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM  LabsCorePopulatedTable 
WHERE PatientID IN (
    SELECT PatientID 
    FROM PatientCorePopulatedTable 
    WHERE PatientLanguage IN ('Icelandic', 'Spanish')
)
LIMIT 100;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT *
FROM  LabsCorePopulatedTable 
WHERE PatientID IN (
    SELECT PatientID 
    FROM PatientCorePopulatedTable 
    WHERE PatientLanguage NOT IN ('Icelandic', 'Spanish')
)
LIMIT 100;
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT *
FROM  LabsCorePopulatedTable 
WHERE PatientID IN (
    SELECT PatientID 
    FROM PatientCorePopulatedTable 
    WHERE PatientLanguage NOT IN ('Icelandic', 'Spanish')
)
LIMIT 100;
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```
