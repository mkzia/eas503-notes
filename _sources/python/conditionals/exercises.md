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


# Exercises

## Exercise 1 
Write a program to prompt the user for temperature in Celsius, convert the temperature to Fahrenheit, and print temperature in Fahrenheit. 


```{code-cell} ipython3
celsius = float(input("What is the Celsius temperature? "))
fahrenheit = 9/5 * celsius + 32
print("The temperature is", fahrenheit, "degrees Fahrenheit.")
```

## Exercise 2
Modify exercise 1 to print a message when F > 90 and F < 32

```{code-cell} ipython3
celsius = float(input("What is the Celsius temperature? "))
fahrenheit = 9/5 * celsius + 32
print("The temperature is", fahrenheit, "degrees Fahrenheit.")

if fahrenheit > 95: # <expr> <relop> <expr>, where <relop> is relation operator
    print("It's really hot out there. Be careful!")

if fahrenheit < 32: # <expr> <relop> <expr>, where <relop> is relation is operator
    print("Be sure to dress warmly!")
```

## Exercise 3 
Write a function to calculate the solution to a quadratic equation.
https://www.mathsisfun.com/algebra/quadratic-equation.html
Prompt the user to the coefficients a, b, and c. 


```{code-cell} ipython3
import math
print("This program finds the real solutions to a quadratic\n")
a = float(input("Enter coefficient a: "))
b = float(input("Enter coefficient b: "))
c = float(input("Enter coefficient c: "))
disc_root = math.sqrt(b * b -4 * a * c)

root1 = (-b + disc_root) / (2 * a)
root2 = (-b - disc_root) / (2 * a)

print("\nThe solutions are:", root1, root2)
```

## Exercise 4 
Modify ex3 to add conditionals to print no real roots, double root, roots


```{code-cell} ipython3
import math
print("This program finds the real solutions to a quadratic\n")
a = float(input("Enter coefficient a: "))
b = float(input("Enter coefficient b: "))
c = float(input("Enter coefficient c: "))
discrim = b * b -4 * a * c

if discrim < 0:
    print("\nThe equation has no real roots!")
else:
    if discrim == 0:
        root = -b / (2 * a)
        print("\nThere is a double root at:", root)
    else:
        disc_root = math.sqrt(b * b - 4 * a * c)
        root1 = (-b + disc_root) / (2 * a)
        root2 = (-b - disc_root) / (2 * a)
        print("\nThe solutions are:", root1, root2)
```

## Exercise 5
Prompt user for three  numbers and find the maximum value


```{code-cell} ipython3
x = int(input("Enter a number: "))
y = int(input("Enter another number: "))
z = int(input("Enter enter a third number: "))

if x1 >= x2 and x1 >= x3:
    maxval = x1
elif x2 >= x1 and x2 >= x3:
    maxval = x2
else:
    maxval = x3

print(maxval)
```

## Exercise 6 
Write a program to prompt the user for their user name. 
If it is 'jdoe', print a log in allowed message; otherwise, print another message


```{code-cell} ipython3
username = 'jdoe'

if username == 'jdoe':
    print('Hi, John.')
else:
    print('Hello, stranger.')
```

## Exercise 7 
Modify ex6 to only allow login if the user is active


```{code-cell} ipython3
active_status = False
username = 'jdoe'

if username =='jdoe' and active_status:
	print('Hi, Jone your are allowed to log in!')
else:
	print('You are not allowed to log in!')

active_status = True
username = 'jdoe'

if username =='jdoe' and active_status:
	print('Hi, Jone your are allowed to log in!')
else:
	print('You are not allowed to log in!')
```


## Exercise 8 
Write a program to prompt the user for name and check if it is john or jane



```{code-cell} ipython3
name = input('What is your name? ')
if name == 'john':
	print('Your name is John!')
elif name == 'jane':
	print('Your name is Jane!')


name = input('What is your name? ')
if name == 'john'.lower():
	print('Your name is John!')
elif name == 'jane'.lower():
	print('Your name is Jane!')
```

## Exercise 9
Write a program that only allows a user to log in if they are a superuser or if they are are teach and active


```{code-cell} ipython3
is_super_user = True
is_teacher = True
is_active = False

if is_super_user or (is_teacher and is_active):
	print('You can access the system')
```

## Exercise 10
Prompt user for age and check if they are 18 or older


```{code-cell} ipython3
age = int(input('What is your age? '))

if age >= 18:
	print('You are older 18 or older')
else:
	print('You are not 18!')
```

## Exercise 11
Prompt the user for two numbers and see if they are equal


```{code-cell} ipython3
number1 = int(input('Please enter a number: '))
number2 = int(input('Please enter a number: '))

if number1 == number2:
	print('Both numbers are equal')

```

## Exercise 12
Prompt user to enter a number and check if it is odd or even


```{code-cell} ipython3
number = int(input('Please enter a number: '))

if number == 0:
	print('The number is even')
elif number % 2 == 0:
	print('The number is even')
else: 
	print('The number is odd')
```