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
		WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'December'
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
		WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'December'
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

```SQL
WITH AGE AS (
	SELECT 
	PATIENTID,
	ROUND((JULIANDAY('NOW') - JULIANDAY(PATIENTDATEOFBIRTH))/365.25) AGE
	FROM 
	PATIENTCOREPOPULATEDTABLE
)
SELECT 
	PATIENTID,
	AGE, 
	CASE 
		WHEN AGE < 18 THEN 'YOUTH'
		WHEN AGE BETWEEN 18 AND 35 THEN 'YOUNG ADULT'
		WHEN AGE BETWEEN 36 AND 55 THEN 'ADULT'
		WHEN AGE >= 56 THEN 'SENIOR'
	END AGERANGE
FROM 
	AGE
ORDER BY AGE
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
WITH AGE AS (
	SELECT 
	PATIENTID,
	ROUND((JULIANDAY('NOW') - JULIANDAY(PATIENTDATEOFBIRTH))/365.25) AGE
	FROM 
	PATIENTCOREPOPULATEDTABLE
)
SELECT 
	PATIENTID,
	AGE, 
	CASE 
		WHEN AGE < 18 THEN 'YOUTH'
		WHEN AGE BETWEEN 18 AND 35 THEN 'YOUNG ADULT'
		WHEN AGE BETWEEN 36 AND 55 THEN 'ADULT'
		WHEN AGE >= 56 THEN 'SENIOR'
	END AGERANGE
FROM 
	AGE
ORDER BY AGE
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
WITH AGE AS (
	SELECT 
	PATIENTID,
	ROUND((JULIANDAY('NOW') - JULIANDAY(PATIENTDATEOFBIRTH))/365.25) AGE
	FROM 
	PATIENTCOREPOPULATEDTABLE
)
SELECT 
	CASE 
		WHEN AGE < 18 THEN 'YOUTH'
		WHEN AGE BETWEEN 18 AND 35 THEN 'YOUNG ADULT'
		WHEN AGE BETWEEN 36 AND 55 THEN 'ADULT'
		WHEN AGE >= 56 THEN 'SENIOR'
	END AGE_RANGE,
	COUNT(*) AGE_RANGE_COUNT
FROM 
	AGE
GROUP BY AGE_RANGE
ORDER BY AGE
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
WITH AGE AS (
	SELECT 
	PATIENTID,
	ROUND((JULIANDAY('NOW') - JULIANDAY(PATIENTDATEOFBIRTH))/365.25) AGE
	FROM 
	PATIENTCOREPOPULATEDTABLE
)
SELECT 
	CASE 
		WHEN AGE < 18 THEN 'YOUTH'
		WHEN AGE BETWEEN 18 AND 35 THEN 'YOUNG ADULT'
		WHEN AGE BETWEEN 36 AND 55 THEN 'ADULT'
		WHEN AGE >= 56 THEN 'SENIOR'
	END AGE_RANGE,
	COUNT(*) AGE_RANGE_COUNT
FROM 
	AGE
GROUP BY AGE_RANGE
ORDER BY AGE
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT
    CAST (
        SUM(
            CASE
                WHEN PatientGender = 'Male'
                AND PatientMaritalStatus = 'Married' THEN 1
                ELSE 0
            END
        ) AS REAL
    ) / CAST (
        SUM(
            CASE
                WHEN PatientGender = 'Female'
                AND PatientMaritalStatus = 'Married' THEN 1
                ELSE 0
            END
        ) AS REAL
    ) MARRIED_MALE_FEMALE_RATIO
FROM
    PatientCorePopulatedTable
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    CAST (
        SUM(
            CASE
                WHEN PatientGender = 'Male'
                AND PatientMaritalStatus = 'Married' THEN 1
                ELSE 0
            END
        ) AS REAL
    ) / CAST (
        SUM(
            CASE
                WHEN PatientGender = 'Female'
                AND PatientMaritalStatus = 'Married' THEN 1
                ELSE 0
            END
        ) AS REAL
    ) MARRIED_MALE_FEMALE_RATIO
FROM
    PatientCorePopulatedTable
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT
    PatientID,
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: SPECIFIC GRAVITY'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: SPECIFIC GRAVITY',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: PH'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: PH',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: RED BLOOD CELLS'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: RED BLOOD CELLS',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: WHITE BLOOD CELLS'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: WHITE BLOOD CELLS'
FROM
    PatientCorePopulatedTable
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    PatientID,
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: SPECIFIC GRAVITY'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: SPECIFIC GRAVITY',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: PH'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: PH',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: RED BLOOD CELLS'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: RED BLOOD CELLS',
    CASE
        WHEN EXISTS(
            SELECT
                1
            FROM
                LabsCorePopulatedTable
            WHERE
                LabsCorePopulatedTable.PatientID = PatientCorePopulatedTable.PatientID
                AND LabsCorePopulatedTable.LabName = 'URINALYSIS: WHITE BLOOD CELLS'
        ) THEN 'Y'
        ELSE 'N'
    END 'URINALYSIS: WHITE BLOOD CELLS'
FROM
    PatientCorePopulatedTable
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


https://www.mountsinai.org/health-library/tests/creatinine-blood-test

```SQL
SELECT
    LabsCorePopulatedTable.PatientID,
    PatientCorePopulatedTable.PatientGender,
    LabName,
    LabValue,
    LabUnits,
    CASE
        WHEN PatientCorePopulatedTable.PatientGender = 'Male'
        AND LabValue BETWEEN 0.7
        AND 1.3 THEN 'Normal'
        WHEN PatientCorePopulatedTable.PatientGender = 'Female'
        AND LabValue BETWEEN 0.6
        AND 1.1 THEN 'Normal'
        ELSE 'Out of Range'
    END Interpretation
FROM
    LabsCorePopulatedTable
    JOIN PatientCorePopulatedTable ON PatientCorePopulatedTable.PatientID = LabsCorePopulatedTable.PatientID
WHERE
    LabName = 'METABOLIC: CREATININE'
ORDER BY
    - LabValue
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    LabsCorePopulatedTable.PatientID,
    PatientCorePopulatedTable.PatientGender,
    LabName,
    LabValue,
    LabUnits,
    CASE
        WHEN PatientCorePopulatedTable.PatientGender = 'Male'
        AND LabValue BETWEEN 0.7
        AND 1.3 THEN 'Normal'
        WHEN PatientCorePopulatedTable.PatientGender = 'Female'
        AND LabValue BETWEEN 0.6
        AND 1.1 THEN 'Normal'
        ELSE 'Out of Range'
    END Interpretation
FROM
    LabsCorePopulatedTable
    JOIN PatientCorePopulatedTable ON PatientCorePopulatedTable.PatientID = LabsCorePopulatedTable.PatientID
WHERE
    LabName = 'METABOLIC: CREATININE'
ORDER BY
    - LabValue
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


```SQL
SELECT
    case
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 10
        AND 12 THEN 'Q4'
    end as Quarter,
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
        WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'December'
    END AdmissionMonth,
    count(*) AddmissionCount
FROM
    AdmissionsCorePopulatedTable
GROUP BY
    Quarter,
    AdmissionMonth
ORDER BY
    strftime('%m', AdmissionStartDate)
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT
    case
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', AdmissionStartDate) BETWEEN 10
        AND 12 THEN 'Q4'
    end as Quarter,
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
        WHEN strftime('%m', AdmissionStartDate) = '12' THEN 'December'
    END AdmissionMonth,
    count(*) AddmissionCount
FROM
    AdmissionsCorePopulatedTable
GROUP BY
    Quarter,
    AdmissionMonth
ORDER BY
    strftime('%m', AdmissionStartDate)
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```










