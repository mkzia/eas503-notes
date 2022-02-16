# Conditionals

## Making Choices/Flow Control/Conditionals

- So far we have only written small programs that are a sequence of instructions. Sometimes you have to alter the sequential flow of a program to suit the needs of a particular situation.
- When strings are compared, they are compared lexicographic, meaning strings are put into alphabetical order and uppercase comes before lowercase.
- The conditional operator converts the conditional into a boolean, which is a basic Python data type.
- `bool` datatype only has two values: `True` or `False`
- There are only three Boolean operators: `and`, `or`, and `not`.

## Truthiness and Falsiness 
- Things that are false on their own
    - `None` (basic data type)
    - `False` (basic data type)
    - Any empty sequence: `''`, `[]`, `()`
    - Any zero value: 0, 0.0
    - Anything whose `len()` returns 0
    - Empty objects
    - Everything else is True 
      

## Boolean Operators

- Assume `a=1` and `b=1`

| Boolean Operators | What it does?                                | Example       |
|----|---------------------------------------------|---------------|
| == | True if a has the same value as b           | a == b #True  |
| != | True if a does not have the same value as b | a != b #False |
| >  | True if a is greater than b                 | a > b # False |
| <  | True if a is less than b                    | a < b # False |
| >= | True if a is greater than or equal to b     | a >= b # True |
| <= | True if a is less than or equal to b        | a <= b # True |

## Logical Operators

| Operator | What it does?                                        | Example                                                                   |
|----------|------------------------------------------------------|---------------------------------------------------------------------------|
| `and`    | True if both a AND b are true (logical conjunction)  | if is_teacher and is_active:   print('You can access')                    |
| `or`     | True if either a OR b are true (logical disjunction) | if is_superuser or (is_teacher and is active):    print('You can access') |
| `not`    | True if the opposite of a is true (logical negation) | if not is_superuser:   print('You cannot access')    



## Examples
```python

not True
not False

True and True
False and False
True and False
False and True


True or True
False or False
True or False
False or True
```


```python
cold = True
windy = False

(not cold) and windy # It is not cold and windy

not (cold and windy) # 
```


## Relational Operators
- `==` Equal to 
- `!=` Not equal to
- `<` Less than
- `>` Greater than
- `<=` Less than or equal to
- `>=` Greater than or equal to

- These operators evaluate to True or False depending on the values you give them.
- Conditionals are used to instruct computer to make a decision. 

```python
45 > 34
45 > 79
45 < 79
45 < 34

23.1 >= 23
23.1 >= 23.1
23.1 <= 23.1
23.1 <= 23

67.3 == 87
67.3 == 67
67.0 == 67
67.0 != 67
67.0 != 23

```
##  Combining Comparisons

```python
x = 2
y = 5
z = 7
x < y and y < z

(x < y) and (y < z) # better

```

```python
x = 3
(1 < x) and (x <= 5)

x = 7
(1 < x) and (x <= 5)


x = 3 
1 < x <= 5 # You can chain comparisons

3 < 5 != True 
(3 < 5) and (5 != True)

3 < 5 != False
(3 < 5) and (5 != False)
```

## String Comparisons
```python
'A' < 'a'
'A' > 'Z'
'abc' < 'abd'
'abc' < 'abcd' # shorter is less

```

```python
'Jan' in '01 Jan 1838'

'Feb' in '01 Jan 1838'

'a' in 'abc'

'A' in 'abc'

```

## Choosing which statements to execute

```Python
if some condition is True:
    do something
elif some other condition is True: # else if 
    do something
elif some other condition is True: # else if 
    do something
elif some other condition is True: # else if 
    do something
else:
    do something 
```
- colons are important and indentation matters
- can have many elif tests
- do not need else
- conditions can be nested



## Example
| pH Level | Solution Category |
|----------|-------------------|
| 0-4      | Strong acid       |
| 5-6      | Weak acid         |
| 7        | Neutral           |
| 8-9      | Weak base         |
| 10-14    | Strong base       |

```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    print(ph, " is acidic.") #indentation important
```

```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    print(ph, " is acidic.")
    print('You should be careful with that!') #indentation is important
```


```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    print(ph, " is acidic.") #indentation important

if ph > 7.0:
    print(ph, " is basic.") 
```

```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    print(ph, " is acidic.") #indentation important
elif ph > 7.0:
    print(ph, " is basic.") 
```

```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    ph = 8.0 #indentation important

if ph > 7.0:
    print(ph, " is basic.") 
```


```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    ph = 8.0  #indentation important
elif ph > 7.0:
    print(ph, " is basic.") 
```


```python
ph = float(input('Enter the pH level: '))
if ph < 7.0:
    ph = 8.0  #indentation important
elif ph > 7.0:
    print(ph, " is basic.") 
```

```python
compound = input('Enter the compound: ')
if compound == 'H20':
    print('Water')
elif compound == 'NH3':
    print('Ammonia')
elif compound == 'CH4':
    print('Methane')
else:
    print('Unknown compound')


```


## Nested if statements
```python
value = input('Enter the pH level: ')
if len(value) > 0:
    ph = float(value) 
    if ph < 7.0:
        print(ph, " is acidic.")
    elif ph > 7.0:
        print(ph, " is basic.")
    else:
        print(ph, " is neutral.")

else:
    print('No pH value was given!')

```


```python
if age < 45:
    if bmi < 22.0:
        risk = 'low'
    else:
        risk = 'medium'
else:
    if bmi < 22.0:
        risk = 'medium'
    else:
        risk = 'high'

```

```python
young = age < 45
slim = bmi < 22.0
if young:
    if slim:
        risk = 'low'
    else:
        risk = 'medium'
else:
    if slim:
        risk = 'medium'
    else:
        risk = 'high'
```

```python
young = age < 45
slim = bmi < 22.0
if young and slim:
    risk = 'low'
elif young and not slim :
    risk = 'medium'
elif not young and slim:
    risk = 'medium'
elif not you and not slim:
    risk = 'high'
```

- What is wrong with the following code?
```python
ph = 2
if ph < 7.0:
    print(ph, ' is acidic')
elif ph < 3.0:
    print(ph, ' is VERY acidic! Be careful.')
```

```python
# Implement the min() function for three inputs

number1 = int(input('Enter first integer: '))
number2 = int(input('Enter second integer: '))
number3 = int(input('Enter third integer: '))

minimum = number1  

if number2 < minimum:
    minimum = number2

if number3 < minimum:
    minimum = number3

print('Minimum value is', minimum)
```


https://www.analyzemath.com/Equations/solve-quadratic-equations-using-discriminants.html
