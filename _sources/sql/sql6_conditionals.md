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

# Conditionals

```
CASE
    WHEN C1 THEN E1
    WHEN C2 THEN E2
    ...
    WHEN CN THEN EN
    [ELSE ED]
END [COLUMN_NAME]
```

```SQL
SELECT 
	CASE
		WHEN strftime('%m', AdmissionStartDate) = '01' THEN 'January'
		WHEN strftime('%m', AdmissionStartDate) = '02' THEN 'February'
		WHEN strftime('%m', AdmissionStartDate) = '03' THEN 'March'
		WHEN strftime('%m', AdmissionStartDate) = '04' THEN 'April'
		WHEN strftime('%m', AdmissionStartDate) = '05' THEN 'May'
		WHEN strftime('%m', AdmissionStartDate) = '06' THEN 'June'
		WHEN strftime('%m', AdmissionStartDate) = '07' THEN 'July'
		WHEN strftime('%m', AdmissionStartDate) = '08' THEN 'August'
		WHEN strftime('%m', AdmissionStartDate) = '09' THEN 'September'
		WHEN strftime('%m', AdmissionStartDate) = '10' THEN 'October'
		WHEN strftime('%m', AdmissionStartDate) = '11' THEN 'November'
		WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'Decemeber'
	END AdmissionMonth,
	count(*) AddmissionCount
FROM 
	AdmissionsCorePopulatedTable
Group BY AdmissionMonth
ORDER BY -AddmissionCount
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
	CASE
		WHEN strftime('%m', AdmissionStartDate) = '01' THEN 'January'
		WHEN strftime('%m', AdmissionStartDate) = '02' THEN 'February'
		WHEN strftime('%m', AdmissionStartDate) = '03' THEN 'March'
		WHEN strftime('%m', AdmissionStartDate) = '04' THEN 'April'
		WHEN strftime('%m', AdmissionStartDate) = '05' THEN 'May'
		WHEN strftime('%m', AdmissionStartDate) = '06' THEN 'June'
		WHEN strftime('%m', AdmissionStartDate) = '07' THEN 'July'
		WHEN strftime('%m', AdmissionStartDate) = '08' THEN 'August'
		WHEN strftime('%m', AdmissionStartDate) = '09' THEN 'September'
		WHEN strftime('%m', AdmissionStartDate) = '10' THEN 'October'
		WHEN strftime('%m', AdmissionStartDate) = '11' THEN 'November'
		WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'Decemeber'
	END AdmissionMonth,
	count(*) AddmissionCount
FROM 
	AdmissionsCorePopulatedTable
Group BY AdmissionMonth
ORDER BY -AddmissionCount
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

