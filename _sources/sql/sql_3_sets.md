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
# Sets

## Basics
SQL supports four types of operations `UNION`, `UNION ALL`, `INTERSECT`, and `EXCEPT`. The difference between `UNION` and `UNION ALL` is that the latter includes duplicates. 
Given set A = {L, M, N, O, P} and set B = {P, Q, R, S, T}, the four operations will return
1. A UNION B  = {L, M, N, O, P, Q, R, S, T}
2. A UNION ALL = {L, M, N, O, P, P, Q, R, S, T} -- Note the two Ps
3. A INTERSECT B = {P}
4. A EXCEPT B = {L, M N, O}

When doing set operations, you should use the same column names so you can order the results. 

## The `UNION ALL`  and `UNION` Operator

```SQL
SELECT 
    'Teacher' typ,
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION ALL
SELECT 
    'Student' typ,
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('students_teachers.db')

sql_statement = """
SELECT 
    'Teacher' typ,
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION ALL
SELECT 
    'Student' typ,
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION ALL
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('students_teachers.db')

sql_statement = """
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION ALL
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION 
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('students_teachers.db')

sql_statement = """
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
UNION
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## The `INTERSECT` Operator

```SQL
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
INTERSECT 
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('students_teachers.db')

sql_statement = """
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
INTERSECT 
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## The `EXCEPT` Operator

```SQL
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
EXCEPT 
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
```


```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('students_teachers.db')

sql_statement = """
SELECT 
    t.first_name
FROM Teachers t
WHERE t.first_name LIKE 'Jo%'
EXCEPT
SELECT 
    s.first_name
FROM Students s
WHERE s.first_name LIKE 'Jo%'
ORDER BY first_name
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```
