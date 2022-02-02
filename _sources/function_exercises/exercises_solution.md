# Function Exercises Solution

## Exercise 1

```python
print('_'*30)
print('Happy birthday to you!!!')
print('Happy birthday to you!!!')
print('Happy birthday, dear John')
print('Happy birthday to you!')
print('Happy birthday to you!')
print('_'*30)
```

## Exercise 2

```python
print('_'*30)
print('Happy birthday to you!')
print('Happy birthday to you!')
print('Happy birthday, dear Jane')
print('Happy birthday to you!')
print('Happy birthday to you!')
print('_'*30)
```

## Exercise 3

```python
def print_happy_birthday():
    print('Happy birthday to you!!!')
    print('Happy birthday to you!!!')
```

## Exercise 4a

```python
print_happy_birthday()
print('Happy birthday, dear John')
print_happy_birthday()
```

## Exercise 4b

```python
print_happy_birthday()
print('Happy birthday, dear Jane')
print_happy_birthday()
```

## Exercise 5

```python
def print_happy_birthday_name(name):
    print(f'Happy birthday, dear {name}')
```

## Exercise 6

```python
print_happy_birthday()
print_happy_birthday_name('John')
print_happy_birthday()
print('-'*80)
print_happy_birthday()
print_happy_birthday_name('Jane')
print_happy_birthday()
```


## Exercise 7

```python
def calculate_distance(x1,y1,x2,y2):
    import math
    distance = math.sqrt((x2-x1)**2 + (y2-y1)**2)

    return distance
```

## Exercise 8

```python
def triple(num):
    return num * 3
```

## Exercise 9

```python
def km_to_miles(km):
    return km / 1.6
```

## Exercise 10

```python
def average_grade(grade1, grade2, grade3):
    return (grade1 + grade2 + grade3) / 3
```

## Exercise 11

```python
def top_three_avg(grade1, grade2, grade3, grade4):
    total = grade1 + grade2 + grade3 + grade4
    top_three = total - min(grade1, grade2, grade3, grade4)
    return top_three / 3
```

## Exercise 12

```python
def weeks_elapsed(day1, day2):
    return int(abs(day1 -day2) / 7)
```