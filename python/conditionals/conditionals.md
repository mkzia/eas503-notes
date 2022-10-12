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

## Previous Chapter
- In the previous chapter we covered how functions. Functions are the fundamental building blocks of writing large programs. 
- We learned to use some built-in function 
- We learned to write our own function using the function design recipe
- We learned how to pass arguments into a function and return values from a function

## Chapter Objectives 
- In this chapter we will cover a fundamental concept in programming: conditionals or making choices using control flow statements. You will learn
  - How to modify the behavior of your program based on the data you are working with
  - What the boolean data type is
  - What truthiness and falsiness of values are
  - What boolean operators are
  - What relational operators are
  - How to combine comparison operators 
  - What is short-circuit evaluation 
  - What it means to compare strings
  - How to write nested if statements

## Conditionals/Making Choices Using Flow Controls
- So far we have only written small programs that are a sequence of instructions. Sometimes you have to alter the sequential flow of a program to suit the needs of a particular situation.

## Boolean Type
- Python has a built-in Boolean type called `bool`. Unlike `int` and `float`, which can have almost unlimited possible values, `bool` has only two: `True` and `False`. `True` and `False` are values just like 0 and -43.7. 

## Boolean Operators
- There are three basic Boolean operators: `and`, `or`, and `not`. `not` has the highest precedence, followed by `and`, followed by `or`. 

| Operator | What it does?                                        | Example                                                                   |
|----------|------------------------------------------------------|---------------------------------------------------------------------------|
| `and`    | True if both a AND b are true (logical conjunction)  | if is_teacher and is_active:   print('You can access')                    |
| `or`     | True if either a OR b are true (logical disjunction) | if is_superuser or (is_teacher and is active):    print('You can access') |
| `not`    | True if the opposite of a is true (logical negation) | if not is_superuser:   print('You cannot access')    


```{code-cell} ipython3
not True
```

```{code-cell} ipython3
not False
```

```{code-cell} ipython3
True and True
```

```{code-cell} ipython3
False and False
```


```{code-cell} ipython3
True and False
```


```{code-cell} ipython3
False and True
```

```{code-cell} ipython3
True or True
```

```{code-cell} ipython3
False or False
```


```{code-cell} ipython3
True or False
```

```{code-cell} ipython3
False or True
```

```python
cold = True
windy = False

(not cold) and windy # It is not cold and windy

not (cold and windy) # 
```


## Truthiness and Falsiness 
- Things that are false on their own
    - `None` (basic data type)
    - `False` (basic data type)
    - Any empty sequence: `''`, `[]`, `()`
    - Any zero value: 0, 0.0
    - Anything whose `len()` returns 0
    - Empty objects
    - Everything else is True 
      
## Relational Operators
- A relational operator converts a conditional into a boolean, which is a basic Python data type.

- Assume `a=1` and `b=1`

| Relational Operators | What it does?                                | Example       |
|----|---------------------------------------------|---------------|
| == | True if a has the same value as b           | a == b #True  |
| != | True if a does not have the same value as b | a != b #False |
| >  | True if a is greater than b                 | a > b # False |
| <  | True if a is less than b                    | a < b # False |
| >= | True if a is greater than or equal to b     | a >= b # True |
| <= | True if a is less than or equal to b        | a <= b # True |

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

## Short-Circuit Evaluation 
- When Python evaluates an expression containing `and` or `or`, it does so from left to right. As soon as it knows enough to stop evaluating, it stops, even if some operands have not been looked at yet. This is called short-circuit evaluation. 

```python
is_superuser = True
is_staff = True
is_active = False
if is_superuser or (is_staff and is_active):
    print('Enter!')
else:
    print('You Shall Not Pass!')
```

``` python
is_staff = True
is_active = True
if is_staff and is_active:
    print('Enter!')
else:
    print('You Shall Not Pass!')
```

## String Comparisons
- When strings are compared, they are compared lexicographic, meaning strings are put into alphabetical order and uppercase comes before lowercase.
- ASCII Table: https://www.rapidtables.com/code/text/ascii-table.html
```python
'A' < 'a'
'A' > 'Z'
'abc' < 'abd'
'abc' < 'abcd' # shorter is less
```
- Convert a single character to its unicode number: `ord()`
- Convert a unicode number to its unicode character: `chr()`


- Python provides a way to check if one string appears inside another one:

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
- can have many `elif` tests
- do not need `else`
- conditions can be nested

### Examples
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

def my_min():
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