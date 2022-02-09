# String Exercises Solution

## Exercise 1
Write a Python function that prints the following:

```python
They'll hibernate during the winter.
```

```python
def string_ex1():
    print('They\'ll hibernate during the winter.')
    print("They'll hibernate during the winter.")
```

## Exercise 2
# Print: "Absolutely not," he said.

print('"Absolutely not," he said.')
print("\"Absolutely not,\" he said.")

## Exercise 3
# Print: "He said, 'Absolutely not,'" recalled Mel.

print("\"He said, 'Absolutely not,'\" recalled Mel.")

## Exercise 4
# Print: left\right.

print("left\\right")


## Exercise 5
# Given x = 3 and y = 12.5 print: The rabbit is 3.
x = 3
y = 12.5

print("The rabbit is {}.".format(x))
print("The rabbit is " + str(x) + '.')

## Exercise 6
# Given x = 3 and y = 12.5 print: The rabbit is 3 years old.
x = 3
y = 12.5

print("The rabbit is {} years old.".format(x))

## Exercise 7
# Given x = 3 and y = 12.5 print: 12.5 * 3.
x = 3
y = 12.5

print("{} * {}.".format(y, x))

## Exercise 8
# Given x = 3 and y = 12.5 print: 12.5 * 3 = 37.5
x = 3
y = 12.5

print("{} * {} = {}.".format(y, x, y*x))

## Exercise 9
# Given x = 3 and y = 12.5 print: 12.5 is average.
x = 3
y = 12.5
print("{} is average.".format(y))