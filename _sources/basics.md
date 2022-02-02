# Basics

## Syntax, Expressions, Values, Operators, and Operands 

- The `syntax` of a computer language is the set of rules that defines the combinations of symbols that are considered to be correctly structured (Wiki).
- An `expression` is a syntactic entity in a programming language that may be evaluated to determine its value (Wiki).
- `2 + 2` 
  - `2` is a value
  - `+` is an operator
  - `2` is a value
  - `2 + 2` is an expression because it expresses the intent of the programmer in a syntax that Python understands.
  - the value and the operator are combined or reduced to 4 -- i.e., the expression 2 + 2 is `evaluated` to 4. In this
  expression, 2 is also called an operand, i.e., the data that is manipulated (Wiki) using operators. 
- Expressions do not have to involve an operator. 
- When an expression is evaluated, it produces a single value. 

## Basic Data Types

- Every value in Python has a particular type, and the types of values determine how they behave when they are combined. 
- You will mostly use these data types: `int`, `float`, `str`, `True/False`, and `None`

## Operators

- `+` Addition -- `2+2=4`
- `-` Subtraction -- `5-2=3`
- `*` Multiplication -- `2*3=6`
- `/` Division -- `12/6=2`
- `**` Exponentiation - raise number to a given power; `2 ^ 3` or `2 * 2 * 2`
  - `64 ** 0.5` is allowed
- `%` Modulo -- remainder operator. Good for figuring out if a number is even or odd
- `//` integer division (i.e. quotient without remainder) `10//3 = 3`

## Operator Notes

- `3/4` is a float in Python! Other languages would cut off the decimal
- By default division results in a float

## Numbers

- Python has the following number data types:
  - int - whole number, positive or negative, unlimited
  - float - number with decimals, positive or negative
  - complex - `7+3j`
- `int` vs `float`
  - Stored differently
  - `float` take up a set amount of space. `int` take up variable amount of space. 
  - `int` are stored as `bignum` data type behind the scenes. 
  
  ```python
  import sys
  sys.getsizeof(2.0)
  Out[3]: 24
  sys.getsizeof(2**30)
  Out[4]: 32
  sys.getsizeof(2**130)
  Out[6]: 44
  ```

  - Both `int` and `float` can store positive and negative numbers
  - `type()` -- used to figure out which type of data type it is
- `float` can only represent approximations to real numbers
  - `2/3` vs `1/3`
- If you do not need fraction values, use int.

## Operator Precedence

- PEMDAS - use parenthesis to manipulate the order in which things are calculated 
  - what is `8/2(2+2)`?
- Precedence
  - Parenthesis
  - Exponentiation
  - Negation
  - Multiplication, division, integer division, and remainder
  - Addition and subtraction
- `212 - 32 * 5 / 9` What is wrong with this expression?

## Working with large numbers
- `10000000000 + 0.00000000001`
- The result should have twenty zeros between the first and last significant digit, but
that is too many for the computer to store, so the result is just 10000000000—it’s as if
the addition never took place. Adding lots of small numbers to a large one can
therefore have no effect at all, which is not what a bank wants when it totals up the
values of its customers’ savings accounts.
- If you have to add up floating-point numbers, add them from smallest to largest in order to minimize the error.
- Better yet, use a specialized library (https://mpmath.org/) or (https://github.com/mdickinson/bigfloat)

### Identifiers, Naming Variables in Python, Remembering Values
- Variables give names to values (number, string, or boolean); Technically they are called identifiers. They are a container of information that a computer program will manipulate using a sequence of instructions. 
- Variables names MUST follow certain rules and it is BEST to follow Python guidelines for naming 
- Restrictions for identifiers or naming things in Python
  1. Start with letter or underscore
  1. The rest can have letters, underscore, and numbers
  1. symbols cannot be used in name (@,+)
  1. Don't use Python keywords or reserved words such as `print`, `str`, `int`, `float`
    - `int = 3` do `del int` to restore python keyword
    - `print = Yah` do `del print` to restore Python keyword
- Conventions for identifiers or naming things in Python
  1. use snake_case not camelCase for variable and function names
  1. variables should be lowercase
  1. upper case are used for constants `PI = 3.14`
  1. UpperCamelCase for classes
  1. `__private__`  double underscore is convention that means you are not supposed access this variable directly. They are by convention like private variables in other languages. 

:::{warning}
Following these conventions in your assignments!
:::


### Working with Variables
- `x = 503`;  
- `y = "EAS503"`
- `print(x)`
- `print(y)`
- `y = 34` this is allowed because you can reassign value of a variable with the new value having a different data type in Python;
This is called Dynamic typing;  C++ statically-typed -- cannot change the variable type. Define variable and variable definition will be enforced

- `x = 3.9 * x * (1 - x)`
- `x = 10`
- `x = 11`
  - A variable in Python is like a sticky note on a value. When you change the variable, the sticky note changes to a different value. The variable simply switches to refer to the new value. The old value is not erased immediately. Python has automatic garbage collection.
- `name = input("What is your name? ")` -- used to prompt user for input; the `name` variable will be a <strong>string!</strong>
  - avoid using `eval` for type casting, meaning changing type of variable from one type to another.
- `sum, diff = x+y, x-y` -- simultaneous assignment 
  - useful when you want to swap values
    - So instead of doing this:

    ``` Python
      temp = x
      x = y
      y = temp
    ```
   -  do:
      - `x, y = y, x`


- `+=` - `x += 3` is the same as `x = x + 3`
- `-=` - `x -= 3` is the same as `x = x - 3`
- `*=` - `x *= 3` is the same as `x = x * 3`
- `/=` - `x /= 3` is the same as `x = x / 3`