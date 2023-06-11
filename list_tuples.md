# Lists and Tuples
- So far we have covered the following data types: `int`, `float`, and `bool`. Now we will
cover `list`, and `tuple` 
- Data types in Python can be classified as `mutable` and `immutable`. 
    - Immutable data types are: `int`, `float`, `bool`, and `tuple`
    - Mutable data types are: `list`, `dict`, and `set`
    - NOTE: In other programming language, data types such as ints and floats are passed to a function using `pass-by-value` and
    data types such as arrays are `pass-by-reference`. This means that changes made to ints and floats inside a function do not change the value outside of the function because they were passed a copy (independent copy). As for arrays, they are passed to a function as a reference, so changes made to the array persist outside the function. 
    - NOTE: In Python, however, everything is passed to a function using `pass-by-object-reference`.
  
    :::{warning}
    Ref: Python for Programmers by Paul Deitel and Harvey Deitel

    In many programming languages there are two ways to pass arguments--pass-by-value
    and pass-by-reference (sometimes called call-by-value and call-by-reference, respectively):
      - With pass-by-value, the called function receives a copy of the argument's value 
      and works exclusively with that copy. Changes to the function's copy do not affect the original
      variable's value in the caller.
      - With pass-by-reference, the called function can access the argument's value in the caller
      directly and modify the value if it's mutable. 

    Python arguments are ALWAYS passed by reference. Some people call this pass-by-object-reference, 
    because "everything in Python is an object." When a function call provides an argument, Python copies
    the argument object's reference--not the object itself--into the corresponding parameter. This
    is important for performance. Function often manipulate large objects--frequency copying them would
    consume large amount of computer memory and significantly slow down program performance. 
    :::


## List
- `list` is a container for storing a sequence of values. The values can be of different type. When the values are all `int`, `float`, or `bool`, you can use special functions. 

### List Index -- starts at 0
```python
whales = [5, 4, 7, 3, 2, 3, 2, 6, 4, 2, 1, 7, 1, 3]
```

### Creating a list
```python
grades = ['A', 'B', 'C', 'D', 'F']
```

### Accessing a list
```python
grades[0]
``` 

### Slicing a list
```python
grades[2:4]
grades[1::2]
grades[-1]
grades[::-1]
```

### Reassigning a list
```python
grades = ['A', 'B', 'C', 'D', 'F']
grades[0] = 'a'
grades[1:2] = 'a'
grades[2:] = ['d', 'f']
```

### Deleting from a list
```python
grades = ['A', 'B', 'C', 'D', 'F']
del grades[0]
del grades[1:3]
del grades
```

### Concatenate lists
```python
grades1 = ['A', 'B', 'C']
grades2 = ['D', 'F']
grades = grades1 + grades2
```

### Multiplication
```python
grades = ['A', 'B', 'C', 'D', 'F']
grades *= 3

```

### Can store different data types
- But you will lose some processing functionality
```python
my_list = ['A', 1, 'Spam', True]
my_list2 = [['John', [55, 65, 86]], ['Jane', [70, 80, 80]]
```

### Built-in List methods
- `len()` calculate length of list
- `max()` calculate max of list
- `min()` calculate min of list
- `sum()` calculate sum of list
- `sorted()` return a sorted list
- `list()` cast to type list -- convert tuple to list or a generator to list
- `any()` return `True` if the truthiness of any value is `True` in the list
- `all()` return `True` if the truthiness of all the values is `True` in the list

```python
numbers = [3, 4, 8, 9, 5, 6, 7, 0, 1, 2, 10, 11, 12]
len(numbers)
max(numbers)
min(numbers)
sum(numbers)
sorted(numbers)
list(numbers)
any(numbers)
all(numbers)
```
```python
booleans = [True, False, True]
any(booleans)
all(booleans)
```

```python
booleans = [True, True, True]
any(booleans)
all(booleans)
```

### List methods (functions)
- `append()` - add an element to end of list
- `insert()` - insert an element to the list at the specified location
- `remove()` - remove the element 
- `pop()` - remove the last element in the list; also returns the value; you can save this to another variable
- `clear()` - empty the list
- `index()` - return position of first matching element
- `count()` - return the number of elements in the list
- `sort()` - sort the list in place 
- `reverse()` - reverse the list in place


```python
grades = ['A', 'B', 'C']
grades.append('D')
grades.insert(4, 'F')
grades.remove(2)
grades.pop()
grades.index('C')
grades.count() # len(grades)
grades.sort()
grades.reverse()
```



## Tuples
- Tuple are just like lists except that they are immutable. Once you have created a tuple, you cannot modify it. 
- Why use them?
  - Faster than lists
  - Make code safer -- because you cannot change it
  - Valid keys in a dictionary


## Writing to a file

```python
with open('test_file.txt', 'w') as file:
    file.write('line1\nline2')
```

```python
title = '|' + '{:^51}'.format('Cereal Yields (kg/ha)') + '|'
line = '+' + '-'*15 + '+' + ('-'*8 + '+')*4
row = '| {:<13} |' + ' {:6,d} |'*4
header = '| {:^13s} |'.format('Country') + (' {:^6d} |'*4).format(1980, 1990,
                                                                  2000, 2010)
file_content = '\n'.join(('+' + '-'*(len(title)-2) + '+',
      title,
      line,
      header,
      line,
      row.format('China', 2937, 4321, 4752, 5527),
      row.format('Germany', 4225, 5411, 6453, 6718),
      row.format('United States', 3772, 4755, 5854, 6988),
      line))

with open('test_file.txt', 'w') as file:
    file.write(file_content)
```