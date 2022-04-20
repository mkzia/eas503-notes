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


# Analytic Functions

Analytic functions allow you to group rows into windows, partitioning
the data. Windows are defined using the `OVER` clause and optionally
combined with the `PARTITION` subclause. 

Date Reference: https://www.techonthenet.com/sqlite/functions/julianday.php

## Data Windows

```SQL
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month,
    sum(total) Monthly_Sales
FROM
    bakery_sales
GROUP BY
    Quarter,
    Month
ORDER BY 
	strftime('%m', sale_date)
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month,
    sum(total) Monthly_Sales
FROM
    bakery_sales
GROUP BY
    Quarter,
    Month
ORDER BY 
	strftime('%m', sale_date)
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
WITH SalesTable AS (
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month, 
	Total
FROM
    bakery_sales
)
SELECT 
	Quarter,
	Month, 
	Sum(Total) MonthlySales,
	Max(sum(total)) over() max_overall_sales,
	Max(sum(total)) over(partition by quarter) max_quarter_sales
FROM SalesTable
GROUP BY Quarter, Month
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
WITH SalesTable AS (
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month, 
	Total
FROM
    bakery_sales
)
SELECT 
	Quarter,
	Month, 
	Sum(Total) MonthlySales,
	Max(sum(total)) over() max_overall_sales,
	Max(sum(total)) over(partition by quarter) max_quarter_sales
FROM SalesTable
GROUP BY Quarter, Month
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## Localized Sorting


```SQL
WITH SalesTable AS (
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month, 
	Total
FROM
    bakery_sales
)
SELECT 
	Quarter,
	Month, 
	Sum(Total) MonthlySales,
	rank() OVER (ORDER BY -sum(total)) SalesRank
FROM SalesTable
GROUP BY Quarter, Month
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
WITH SalesTable AS (
SELECT
    CASE
        WHEN 0 + strftime('%m', sale_date) BETWEEN 1
        AND 3 THEN 'Q1'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 4
        AND 6 THEN 'Q2'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 7
        AND 9 THEN 'Q3'
        WHEN 0 + strftime('%m', sale_date) BETWEEN 10
        AND 12 THEN 'Q4'
    END Quarter,
    CASE
        WHEN strftime('%m', sale_date) = '01' THEN 'January'
        WHEN strftime('%m', sale_date) = '02' THEN 'February'
        WHEN strftime('%m', sale_date) = '03' THEN 'March'
        WHEN strftime('%m', sale_date) = '04' THEN 'April'
        WHEN strftime('%m', sale_date) = '05' THEN 'May'
        WHEN strftime('%m', sale_date) = '06' THEN 'June'
        WHEN strftime('%m', sale_date) = '07' THEN 'July'
        WHEN strftime('%m', sale_date) = '08' THEN 'August'
        WHEN strftime('%m', sale_date) = '09' THEN 'September'
        WHEN strftime('%m', sale_date) = '10' THEN 'October'
        WHEN strftime('%m', sale_date) = '11' THEN 'November'
        WHEN strftime('%m', sale_date) = '12' THEN 'December'
    END Month, 
	Total
FROM
    bakery_sales
)
SELECT 
	Quarter,
	Month, 
	Sum(Total) MonthlySales,
	rank() OVER (ORDER BY -sum(total)) SalesRank
FROM SalesTable
GROUP BY Quarter, Month
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```


## Window Frames

ref: https://www.sqlitetutorial.net/sqlite-window-functions/sqlite-window-frame/

```SQL
WITH SalesTable AS (
SELECT
	cast(strftime('%Y', sale_date) AS INT) sale_year,
	cast(strftime('%W', sale_date) AS INT) sale_week,
	Total
FROM
    bakery_sales
ORDER BY sale_date
)
SELECT 
	sale_year,
	sale_week,
	sum(total) week_total,
	sum(sum(total)) OVER (ORDER BY sale_week ROWS UNBOUNDED PRECEDING) rolling_sum
FROM SalesTable
GROUP BY sale_year, sale_week
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
WITH SalesTable AS (
SELECT
	cast(strftime('%Y', sale_date) AS INT) sale_year,
	cast(strftime('%W', sale_date) AS INT) sale_week,
	Total
FROM
    bakery_sales
ORDER BY sale_date
)
SELECT 
	sale_year,
	sale_week,
	sum(total) week_total,
	sum(sum(total)) OVER (ORDER BY sale_week ROWS UNBOUNDED PRECEDING) rolling_sum
FROM SalesTable
GROUP BY sale_year, sale_week
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
SELECT 
	sale_date,
	sum(total) total,
	max(sum(total)) over (order by cast(strftime('%j', sale_date) AS INT) range between 3 preceding and 3 following) seven_day_max
FROM bakery_sales
GROUP BY sale_date
order by sale_date
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
SELECT 
	sale_date,
	sum(total) total,
	max(sum(total)) over (order by cast(strftime('%j', sale_date) AS INT) range between 3 preceding and 3 following) seven_day_max
FROM bakery_sales
GROUP BY sale_date
order by sale_date
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```
## Lag and Lead


```SQL
WITH SalesTable AS (
SELECT
	cast(strftime('%Y', sale_date) AS INT) sale_year,
	cast(strftime('%W', sale_date) AS INT) sale_week,
	Total
FROM
    bakery_sales
ORDER BY sale_date
)
SELECT 
	sale_year,
	sale_week,
	sum(total) week_total,
	lag(sum(total), 1) over (order by sale_week) prev_week_total,
	lead(sum(total), 1) over (order by sale_week) next_week_total
FROM SalesTable
GROUP BY sale_year, sale_week
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
WITH SalesTable AS (
SELECT
	cast(strftime('%Y', sale_date) AS INT) sale_year,
	cast(strftime('%W', sale_date) AS INT) sale_week,
	Total
FROM
    bakery_sales
ORDER BY sale_date
)
SELECT 
	sale_year,
	sale_week,
	sum(total) week_total,
	lag(sum(total), 1) over (order by sale_week) prev_week_total,
	lead(sum(total), 1) over (order by sale_week) next_week_total
FROM SalesTable
GROUP BY sale_year, sale_week
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

```SQL
WITH SalesTable AS (
    SELECT
        cast(strftime('%Y', sale_date) AS INT) sale_year,
        cast(strftime('%W', sale_date) AS INT) sale_week,
        Total
    FROM
        bakery_sales
    ORDER BY
        sale_date
)
SELECT
    sale_year,
    sale_week,
    sum(total) week_total,
    round(
        CAST(
            (
                sum(total) - lag(sum(total), 1) over (
                    order by
                        sale_week
                )
            ) AS REAL
        ) / CAST(
            lag(sum(total), 1) over (
                order by
                    sale_week
            ) AS REAL
        ) * 100,
        1
    ) CHANGE
FROM
    SalesTable
GROUP BY
    sale_year,
    sale_week
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('bakery_sales.db')

sql_statement = """
WITH SalesTable AS (
    SELECT
        cast(strftime('%Y', sale_date) AS INT) sale_year,
        cast(strftime('%W', sale_date) AS INT) sale_week,
        Total
    FROM
        bakery_sales
    ORDER BY
        sale_date
)
SELECT
    sale_year,
    sale_week,
    sum(total) week_total,
    round(
        CAST(
            (
                sum(total) - lag(sum(total), 1) over (
                    order by
                        sale_week
                )
            ) AS REAL
        ) / CAST(
            lag(sum(total), 1) over (
                order by
                    sale_week
            ) AS REAL
        ) * 100,
        1
    ) CHANGE
FROM
    SalesTable
GROUP BY
    sale_year,
    sale_week
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```

## Ranking

`rank`: returns the same ranking in case of a tie, with gaps in the rankings
`row_number`: returns a unique number for each row, with rankings arbitrarily assigned in case of a tie
`dense_rank`: returns the same ranking in the case of a tie, with no gaps in the rankings

Ref: https://blog.jooq.org/the-difference-between-row_number-rank-and-dense_rank/


```SQL
SELECT 
	PatientGender,
	PatientMaritalStatus, 
	count(*) StatusCount, 
	rank() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusRank,
	row_number() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusRowNumber,
	dense_rank() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusDenseRank
FROM 
PatientCorePopulatedTable
GROUP BY PatientGender, PatientMaritalStatus
```

```{code-cell} ipython3
:tags: ["hide-input", "output_scroll"]
import sqlite3
import pandas as pd

conn = sqlite3.connect('100_patients.db')

sql_statement = """
SELECT 
	PatientGender,
	PatientMaritalStatus, 
	count(*) StatusCount, 
	rank() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusRank,
	row_number() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusRowNumber,
	dense_rank() OVER (PARTITION BY PatientGender ORDER BY COUNT(*) DESC) StatusDenseRank
FROM 
PatientCorePopulatedTable
GROUP BY PatientGender, PatientMaritalStatus
"""
df = pd.read_sql_query(sql_statement, conn)
df.style.set_table_attributes('style="font-size: 12px"')
```