# Miscellaneous Topics
- Topics
  - Counter
  - Sorting 
    - natural sorting
    - operator
  - Decorators
  - Regular Expressions

## Counter
Python provides some specialized container data types. We have used `defaultdict`. Now we will 
learn about another useful collection called `Counter`. This collection takes in a list of elements
and returns a dictionary with the element as keys and their counts as values. 

```python
from collections import Counter
my_list = ['apple', 'apple', 'banana', 'pineapple']
c = Counter(my_list)
>>> Counter({'apple': 2, 'banana': 1, 'pineapple': 1})
```

```python
from collections import Counter

import random
random.seed(503)

two_dices = []
for _ in range(1000):
    dice1 = random.randint(1,6)
    dice2 = random.randint(1,6)
    two_dices.append(dice1+dice2)
c = Counter(two_dices)
```

## Sorting
itemgetter accesses elements using `[]`. 

```python
import operator
x = ((1, 'a', 3.5), (3, 'Z', 90))
sorted(x, key=operator.itemgetter(2))
```

```python
x = [
    {'name': 'student2', 'test1': '100', 'test2': '90','test3': '65', 'test4': '68', 'test5': '94'},
    {'name': 'student83', 'test1': '88', 'test2': '76', 'test3': '65', 'test4': '97', 'test5': '82'},
    {'name': 'student31', 'test1': '90', 'test2': '87', 'test3': '66', 'test4': '95', 'test5': '72'}]

sorted(x, key=operator.itemgetter('name'))
sorted(x, key=operator.itemgetter('test1'))
```



```python
data = (
    ('AMD', 'Advanced Micro Devices, Inc.',
     '$120.88', '+4.27', '121,475,927'),
    ('AAPL', 'Apple Inc.', '$164.71', '+1.97', '80,725,613'),
    ('OPEN', 'Opendoor Technologies Inc', '$8.42', '-2.55', '53,559,847'),
    ('NVDA', 'NVIDIA Corporation', '$240.82', '+3.34', '49,046,544'),
    ('ZNGA', 'Zynga Inc.', '$9.18', '+0.37', '48,193,380'),
)
sorted(data, key=operator.itemgetter(3))
```

```python
from natsort import natsorted
natsorted(data, key=lambda ele: ele[3], reverse=True)
```