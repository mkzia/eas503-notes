# Lambda, Filter, Map, and Sorted Solutions

## Exercise 1
Write a function that squares a number and returns the value.
Write it again again using Lambda functions

```python
def square(num):
    return num * num

square2 = lambda num: num * num

print(square(9))
print(square2(9))
```

## Exercise 2
Write a lambda function for adding two numbers

```python
add = lambda a,b: a + b
print(add(3,10))
```

## Exercise 3
Write a lambda function that multiples two numbers. Use the lambda function as an input to another function. 

```python
func = lambda a, b: a * b
def lambda_test(function, number1, number2):
    return function(number1, number2)
print(lambda_test(func, 3, 4))
```

## Exercise 4
1) Write a function to double a list
2) Use list comprehension to double a list
3) Use lambda with map to double a list

map objects are a generator. How many times can you iterate over them?

```python
my_list = [2, 3, 4]

# double this list

# method 1 - define a square function

def square_list(my_list):
    new_list = []
    for ele in my_list:
        new_list.append(ele * ele)

    return new_list

print(square_list(my_list))

# method 2 - list comprehension

print([ele*ele for ele in my_list])

# method 3 - use lambda with map
map(lambda variables (comma separated): expression)

print(list(map(lambda ele: ele*ele, my_list)))


# map objects are a generator. How many times can you iterate over them?
```


## Exercise 5
Capitalize the names in the students list 
1) using a list and loop
2) using list comprehension
3) using lambda and map

```python
students = ['john', 'jane', 'doe']
# make upper case


# method 1 create new list and loop
new_list = []
for student in students:
    new_list.append(student.title())

print(new_list)

# method 2 -- use list comprehension

print([student[0].upper()+student[1:] for student in students])

# method 3 -- use lambda

print(list(map(lambda student: student.title(), students)))

```

## Exercise 6
Using lambda and map, convert the numbers to float

Covert each value to float
```python
my_dict = [{'value': '34.4'}, {'value': '45.3'}, {'value': '73.4'}]

print(list(map(lambda ele: {'Value': float(ele['value'])}, my_dict)))


```

## Exercise 7
Create a dictionary where the key is year and 
value is True/False if the year is a leap year
1) using a loop and dict
2) using filter without a function

```python
years = range(1970, 2000) # for these years



def is_leap_year(year):
    # does not work for century year
    if year % 4 == 0:
        return True
    else:
        return False

my_dict = {}
leap_years = []
for year in years:
    my_dict.update({year: is_leap_year(year)})
    if is_leap_year(year):
        leap_years.append(year)

import pprint
pprint.pprint(my_dict)
print(leap_years)

# filter(function, sequence)

leap_year_lambda = lambda year: year % 4 == 0 

filter(leap_year_lambda, range(1970, 2000))

print(list(filter(lambda year: year % 4 == 0, range(1970, 2000))))
print(list(filter(lambda year: is_leap_year(year), range(1970, 2000))))
print(list(map(lambda year: year/5, filter(lambda year: year % 4 == 0, range(1970, 2000)))))

# # combining map with filter

print(list(map(lambda year: f'{year} is a leap year!', filter(lambda year: year % 4 == 0, range(1970, 2000)) )))


# # list comprehension

# print()
print([f'{year} is a leap year' for year in range(1970, 2000) if year % 4 == 0])

```

## Exercise 8
Sort `x` using value

```python
x = (('efg', 1), ('abc', 3), ('hij', 2))

print(sorted(x)) # does work

print(sorted(x, key=lambda ele: ele[1]))


```
## Exercise 9
sort dictionary by username; reverse sort

```python
students = [
    {'username': 'john', 'grade': 50},
    {'username': 'jane', 'grade': 80},
    {'username': 'doe', 'grade': 35},
    {'grade': 89, 'username': 'Kelly'}
]

from pprint import pprint
print(sorted(students, key=lambda student: student['username']))
print()
print(sorted(students, key=lambda student: student['username'], reverse=True))

```

## Exercise 10
Sort dictionary by grade; reverse sort

```python
students = [
    {'username': 'john', 'grade': 50},
    {'username': 'jane', 'grade': 80},
    {'username': 'doe', 'grade': 35},
    {'grade': 89, 'username': 'Kelly'}
]

print()
print(sorted(students, key=lambda student: student['grade']))
print()
print(sorted(students, key=lambda student: student['grade'], reverse=True))
```

## Exercise 11
Sort array by len of names

```python
students = ['john', 'Janette', 'doe']
print(min(students))
print(min(students, key=lambda student: len(student)))
print(max(students, key=lambda student: len(student)))
```

## Exercise 12
Sort dictionary by value

```python
my_list = [{'value': '34.4'}, {'value': '45.3'}, {'value': '73.4'}]
print(max(my_list, key=lambda ele: float(ele['value'])))
```

