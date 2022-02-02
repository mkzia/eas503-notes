# Functions

- What is a function? https://www.mathsisfun.com/sets/function.html
- Functions are subprograms -- they are a sequence of of statements that have a name
- Functions can be executed at any point by using their name 
- Functions remove duplicated code
- Functions can call other functions
- Functions can OPTIONALLY take parameters or inputs that they can use inside the function 

## Built-in functions
- `abs(-9)` -- `-9` is the input argument. Arguments appear between the parenthesis after the function name

```python
day_temperature = 3
night_temprature = 10
abs(day_temperature - night_temprature)
```

Because function calls produce values, they can be used in expressions:

```python
abs(-7) + abs(3.3)
```

Functions to convert one type of variable to another

```python
int(34.6)
int(-4.3)
float(21)
str(21)
```

The Round function can round floats

```python
round(3.8)
round(3.3)
round(3.5)
round(-3.3)
round(-3.5)
```

The round function can take an OPTIONAL second argument

```python
round(3.141592653,2)
```

The `help(fxn)` function gives information about a function

```python
help(round)
help(pow)
```

Using the `pow` function

```python
pow(2, 4)
pow(2, 4, 3)
```

We can also use function calls as arguments to other functions:

```python
pow(abs(-2), round(4.3))
```

You can use the `id()` function to find the memory address of objects. 

```python
id(-9)
id(23.1)
shoe_size = 8.5
id(show_size)
```

Some other useful functions

```python
min(2, 3, 4)
max(2, -3, 4, 7, -5)
max(2, -3, min(4, 7), -5)

```

## Defining your own functions
- The build-in functions that Python provides do basic tasks. We can write our own functions that can execute complicated sequence
of instructions. Your functions will be made up of Python operators and built-in Python functions.

### Designing New Functions: A Recipe
- What do you name the function?
- What are the parameters, and what types of information do they refer to?
- What calculations are you doing with that information?
- What information does the function return?
- Does it work like you expect it to? -- We will not cover in this class. 


```python
def f(x):
    squared_x  = x * x
    return squared_x
```

```python
def f(x):
    squared_x = x ** 2
    return squared_x
```

```python
def f(x):
    squared_x = pow(x, 2)
    return squared_x
```

```python
def f(x):
    return x**2
```

```python
def f(x):
    return pow(x, 2)
```


### Variations in functions
- No input; no output; example -- print something
- One or more input; no output; example -- print the input
- One or more input: one or more output; example -- take two numbers and return their sum
- No input; one or more output; example -- a random number

### Function Exercises

- Function Exercises 1-12