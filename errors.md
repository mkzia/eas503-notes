# Errors and Exception Handling

Python raises many different types of errors to inform you of what went wrong. The following are the most common errors that you should know. 

## SyntaxError -- invalid Python syntax

```python
def func(()):
   pass

for index in some_list
    print(index)
```

## NameError -- using a variable before it has been defined

```python
def func():
   print(variable)
```

## TypeError -- mismatch data type

```python
'3' + 3
```

## IndexError -- accessing an index that does not exist

```python
my_list = [1, 2, 3]
my_list[4]
```

## ValueError -- calling a built-in function with invalid value type

```python
int('3')
int('3a')
```

## KeyError -- like by index error for lists

```python
student = {'username': 'john', 'grade': 50}
student['class']
```

## AttributeError -- missing attribute -- variables or methods

```python
class TestClass:
    class_variable1 = 2

print(TestClass.class_variable1)
print(TestClass.class_variable2)

string = 'test1234'
string.upper()

x = [1,2,3]
x.upper()
```

## More errors
For more errors see https://docs.python.org/3/library/exceptions.html


## Raising an error
You can raise an error to indicate to the user that something is wrong.

### Raising error -- with a message or without message

```python
raise ValueError('a message')
raise ValueError
```

```python
def calculate_bmi(height, weight):

    if type(height) not in [float, int]:
        raise TypeError('Height has to be float or an int')

    if type(weight) not in [float, int]:
        raise TypeError('Weight has to be float or an int')


    if height <= 0:
        raise ValueError('Height cannot be less than or equal to 0')

    if weight <= 0:
        raise ValueError('Weight cannot be less than or equal to 0')

    return 703 * weight / height**2

```

## Exception Handling

You can gracefully catch errors that are raised and handle them using 
the `try/except` block. 
Try and Except blocks -- a way to handle errors
When you raise an error, your program exits. What
you rather want to do is to catch that error and give
user feedback.


```python
try:
    statements              # Run this main action first
except name1:
    statements              # Run if exception name1 is raised during try block
except (name2, name3):
    statements              # Run if any of these exceptions occur
except name4 as var:
    statements              # Run if exception name4 is raised, assign instance raised to var
except:
    statements              # Run for all other exceptions raised
else:
    statements              # Run if no exception was raised during try block
finally:
    statements              # Always run
```

### Example1
```python
while True:
    try:
        height = float(input('Please enter height: '))
        weight = float(input('Please enter weight: '))
    except Exception as error:
        print(error)
        print('Please enter a number')
    else:
        print('I run when there is no error')
        break
    finally:
        print('I always run!')

print(calculate_bmi(height, weight))
```

### Example2
```python
student = {'username': 'john', 'grade': 50}

def get_key(some_dict, key, default_value=None):
    try:
        return some_dict[key]
    except KeyError:
        return default_value


print(get_key(student, 'username1', default_value='Error'))
print(get_key(student, 'username1'))
```

### Example3 -- combining exceptions

```python
filename = 'abcd.txt'
try: 
    file = open(filename) 
except OSError: 
    print('OS error') 
except FileNotFoundError: 
    print('File not found') 
```