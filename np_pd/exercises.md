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

# Cross-referenced Exercises

## Query Primer

### Basic query
SQL cross-reference: {ref}`sql:primer:basic`

```{code-cell} ipython3
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
df
```

### Limit rows
SQL cross-reference: {ref}`sql:primer:limit_rows`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')

# Get first 10 rows
display(df.head(10))

# Get last 10 row
display(df.tail(10))

# Get rows between range
display(df[50:60])
```

### Select some columns
SQL cross-reference: {ref}`sql:primer:select_some_columns`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')

df[['PatientID', 'PatientDateOfBirth']].head(10)
```

### Using column alias
SQL cross-reference: {ref}`sql:primer:using_column_alias`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')

df[['PatientID', 'PatientDateOfBirth']].rename(columns={'PatientDateOfBirth': 'Date of Birth'}).head(10)
```

### Adding columns not from the table
SQL cross-reference: {ref}`sql:primer:adding_column`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
df_new = df[['PatientID', 'PatientPopulationPercentageBelowPoverty']].copy()
df_new.rename(
    columns={
        'PatientID': 'PTID', 
        'PatientPopulationPercentageBelowPoverty': 'Poverty Level'
    }, 
    inplace=True
)
df_new.insert(loc=1, column='Hospital', value='Buffalo Hospital')
df_new['Poverty Level'] = df_new['Poverty Level'] * 10
df_new.head(10)
```

### Removing duplicates
SQL cross-reference: {ref}`sql:primer:removing_duplicates`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
pd.DataFrame(df['PatientMaritalStatus'].unique(), columns=['PatientMaritalStatus'])
```

### Removing duplicates with multiple columns
SQL cross-reference: {ref}`sql:primer:removing_duplicates_2`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
df[['PatientRace', 'PatientMaritalStatus']].drop_duplicates().sort_values(['PatientRace', 'PatientMaritalStatus']).reset_index(drop=True)
```

### Derived table
SQL cross-reference: {ref}`sql:primer:derived_table`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'AdmissionsDiagnosesCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
pd.DataFrame(df[['PrimaryDiagnosisCode', 'PrimaryDiagnosisDescription']].apply(lambda row: f'({row[0]}) {row[1]}', axis=1), columns=['CodeWDescription'])

```


### Filtering Data
SQL cross-reference: {ref}`sql:primer:where_clause`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
mask = (
    (((df['PatientRace'] == 'White') & (df['PatientMaritalStatus'] == 'Married')) |
    ((df['PatientRace'] == 'African American') & (df['PatientMaritalStatus'] == 'Married'))) &
    (df['PatientPopulationPercentageBelowPoverty'] > 15)
)
df_new = df[mask][['PatientID', 'PatientRace', 'PatientMaritalStatus', 'PatientPopulationPercentageBelowPoverty']].reset_index(drop=True)
df_new
```

### Sort Values
SQL cross-reference: {ref}`sql:primer:order_by_clause`

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
df_new = df[
    ['PatientMaritalStatus', 'PatientPopulationPercentageBelowPoverty']
].sort_values(['PatientPopulationPercentageBelowPoverty']).reset_index(drop=True)
display(df_new)
```

```{code-cell} ipython3
from IPython.display import display
import pandas as pd
import numpy as np
filename = 'PatientCorePopulatedTable.txt'

df = pd.read_csv(filename, delimiter='\t')
df_new = df[
    ['PatientMaritalStatus', 'PatientPopulationPercentageBelowPoverty']
].sort_values(['PatientMaritalStatus', 'PatientPopulationPercentageBelowPoverty'], ascending=[True, False]).reset_index(drop=True)
display(df_new)
```

## Filtering

### Conditional Evaluation 
