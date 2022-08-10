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


# Strings

## Previous Lecture
- In the previous lecture we covered functions.
  - Functions are small, self-contained set of instructions that can be called at anytime.
- We covered how to use built-in functions.
- We covered how to write your own functions


## Lecture Objectives
- In this lecture we will take a deep-dive into the str or string data type. You will learn
  - What is the meaning of arithmetic operators on strings.
  - How to convert strings to numbers and numbers to strings.
  - How to use special characters inside strings.
  - How to print results to the screen.
  - How to get information from the user. 
  - How to format strings.
  - What methods are available to manipulate strings.


## Strings Basics

- In Python, text is represented as a string, which is a sequence of characters (letters, digits, and symbols).
- In Python, we indicate that a value is a string by putting either single or double quotes around it.

### Operations on Strings
- `len()` -- to get length of a string
- `+` -- can add strings, but not str and type float or int
- `*` -- to repeat a string 
- `+=` -- add to another string and save value
- `int(3)`
- `float(3.4)`

### Simple examples

#### Single Quotes vs Double Quotes
-
```python
'Aristotle'
"Issac Newton"
```

#### Triple Quotes for String Literal
```python
'''Should you want a string
​ 	that crosses multiple lines,
​ 	Use matched triple quotes.'''
```

```python
"""
This is a string literal. 
        Spaces are printed as is. 
  Hello there!"""
```

#### Determining the length of a string
```python
scientist = 'Issac Newton'
print(len(scientist))
```


#### Concatenating Strings
```python
'Alan Turning' + ' ' + 'Grace Hopper'
```

```python
'NH' + 3 # This will not work. You cannot add a str and int type. 
```

```python
'Four score and ' + str(7) + ' years ago'
```

#### Converting Strings to Numbers
```python
int('0')
int('11')
int('-324')
float('-324.40')
```

#### Repeating Strings
```python
'AT' * 5
'-' * 5
```

## Using Special Characters in Strings
- How would you put a single quote inside a string that is declared using a single quote?
- `'\\'` -- how would you print `/\/\`
- `'\n'`
- `'\''`
- `'\"'`
- `'\t'` -- useful for parsing TSV files



## Printing in Python
```python
print('abbcd', 2, 3)
print('abbcd', 2, 3, sep='\n') # default separator is space
```


## Getting information from the Keyboard
```python
number = input('Please enter a number: ')
```

### Converting Numeric Values
```python
number = int(input('Please enter a number: '))
```

```python
def convert_celsius(fahrenheit):
    return (fahrenheit - 32.0) * 5.0 / 9.0    
```

```python
def convert_celsius():
    fahrenheit = float(input('Please enter temperature in Fahrenheit: '))
    celsius =  (fahrenheit - 32.0) * 5.0 / 9.0  
    print('The temperature in celsius is: ', celsius) 

```

## String Formatting
- The fastest and latest way to do string formatting is using the F-strings (https://realpython.com/python-f-strings/)
- ``` python
  PI = 3.14159265359 
  print(f'{PI:.2f}')
  ```
- But you have to know the other ways so you can read older code or use these ways if you have to use an older version of Python

### Oldest Method
 ``` python
  PI = 3.14159265359 
  name = 'PI'
  print('%s is %.2f' % (name, PI))  # oldest way format specifier is <width>.<precision><type>
```

### Newer Method
- Still used for creating string templates
``` python
PI = 3.14159265359 
name = 'PI'
#{<index>:<format-specifier>} where the format specifier is <width>.<precision><type>
print(('{0} is {1:.2f}'.format('PI', PI)) ) # 
```

### Newest Method
- Newest and fastest method for string formatting
``` python
PI = 3.14159265359 
name = 'PI'
# {<name_of_variable>:<format-specifier>} where the format specifier is <width>.<precision><type>
print(f'{name} is {PI:.2f}') # newest way
```

Reference: https://pyformat.info/

```python
course_number = 'EAS503'
class_size = 113
class_average = 92.3

my_str * 10

x = '-'
x * 10

my_str = 'EAS503'
my_int = 113
my_float = 92.3

line1 = 'This is the first line.\n'
line2 = 'This is the second line.'
lines = line1 + line2

line = 'This is the first line.\n'
line += 'This is the second line.'
```


### Example 1

```python
str_format = '{}'.format(course_number)
f_string = f'{course_number}'

print(str_format)
print(f_string)
```

### Example 2

```python
str_format = 'The course number is {}.'.format(course_number)
f_string = f'The course number is {course_number}.'

print(str_format)
print(f_string)
```


### Example 3 use index

```python
str_format = 'The course number is {}. It has {} students.'.format(course_number, class_size)
str_format = 'The course number is {0}. It has {1} students.'.format(course_number, class_size)
f_string = f'The course number is {course_number}. It has {class_size} students.'

print(str_format)
print(f_string)
```


### Example 4 change index

```python
str_format = 'The course number is {1}. It has {0} students.'.format(course_number, class_size)
f_string = f'The course number is {class_size}. It has {course_number} students.'

print(str_format)
print(f_string)
```

### Example 5 adding a float

```python
str_format = 'The course number is {0}. It has {1} students. The class average is {2}'.format(course_number, class_size, class_average)
f_string = f'The course number is {class_size}. It has {course_number} students. The class average is {class_average}.'

print(str_format)
print(f_string)
```

### Example 6 specify number of spaces to use -- width

```python
str_format = 'The course number is {0:10}. It has {1:10} students. The class average is {2:10}'.format(course_number, class_size, class_average)
f_string = f'The course number is {course_number:10}. It has {class_size:10} students. The class average is {class_average:10}.'

print(str_format)
print(f_string)
```

### Example 7 right align


```python
str_format = 'The course number is {0:>10}. It has {1:>10} students. The class average is {2:>10}'.format(course_number, class_size, class_average)
f_string = f'The course number is {class_size:>10}. It has {class_size:>10} students. The class average is {class_average:>10}.'

print(str_format)
print(f_string)
```

### Example 8 left align

```python
str_format = 'The course number is {0:<10}. It has {1:<10} students. The class average is {2:<10}'.format(course_number, class_size, class_average)
f_string = f'The course number is {course_number:<10}. It has {class_size:<10} students. The class average is {class_average:<10}.'

print(str_format)
print(f_string)
```


### Example 9 center align

```python
str_format = 'The course number is {0:<10}. It has {1:<10} students. The class average is {2:<10}'.format(course_number, class_size, class_average)
f_string = f'The course number is {course_number:<10}. It has {class_size:<10} students. The class average is {class_average:<10}.'

print(str_format)
print(f_string)
```

### Example 10 Padding with zeros

```python
student_id = 223333

str_format = 'The number padded {} padded with zeros {:08}'.format(student_id, student_id)
f_string = f'The number padded {student_id} padded with zeros {student_id:08}'

print(str_format)
print(f_string)
```

### Example 11 Padding with dashes

```python
student_id = 223333

str_format = 'The number padded {} padded with zeros {:->8}'.format(student_id, student_id)
f_string = f'The number padded {student_id} padded with zeros {student_id:->8}'

print(str_format)
print(f_string)
```


https://scipython.com/book/chapter-2-the-core-python-language-i/string-representation-of-integers-with-comma-separated-thousands/
```python
title = '|' + '{:^51}'.format('Cereal Yields (kg/ha)') + '|'
line = '+' + '-'*15 + '+' + ('-'*8 + '+')*4
row = '| {:<13} |' + ' {:6,d} |'*4
header = '| {:^13s} |'.format('Country') + (' {:^6d} |'*4).format(1980, 1990,
                                                                  2000, 2010)
print('+' + '-'*(len(title)-2) + '+',
      title,
      line,
      header,
      line,
      row.format('China', 2937, 4321, 4752, 5527),
      row.format('Germany', 4225, 5411, 6453, 6718),
      row.format('United States', 3772, 4755, 5854, 6988),
      line,
      sep='\n')
```



## String methods
- We have already encountered functions: built-in functions and functions we have defined. A method is another kind of function that is attached to a particular type. This section covers
the methods that are attached to string types.  
- Method calls in this form—`'browning'.capitalize()`—are shorthand for this: `str.capitalize('browning')`. 


![String Methods](string_methods1.png)
![String Methods](string_methods2.png)


## Summary
- Python uses type str to represent text as sequences of characters.
- Strings are created by placing pairs of single quotes `’` or double quotes `"` around the text.
```python
'''Should you want a string
    ​ 	that crosses multiple lines,
    ​ 	Use matched triple quotes.'''
```
- Special characters like newline (\n) and tab (\t) are represented using escape sequences that begin with a backslash. For example, `"this string\nspans\nthree lines"`.
- Values can be printed using built-in function print, and input can be provided by the user using built-in function input. 
- Methods are like functions, except that the first argument must be an object of the class in which the method is defined.
