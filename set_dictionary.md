# Sets and Dictionaries

- Formal mathematical sets
- Do not have duplicate values
- Are not ordered
- Cannot access via index. 
- useful for doing set operations

```python
A = {0, 2, 4, 6, 8}
B = {1, 2, 3, 4, 5}
```

## Check if value belongs
```python
2 in A
```

## Union -- all values
```python
A | B
```
## Intersection -- shared values
```python
A & B
```


## Difference -- order matters
```python
A - B
B - A
```


## Symmetric difference  
https://en.wikipedia.org/wiki/Symmetric_difference

```python
A ^ B
my_list = [1, 1, 2, 3]
my_list = list(set(my_list))
```


# Dictionaries
- limitations of list
- list has collection of values, but no description
- student
  - name
  - email 
  - id
  - major
- dictionary
- key value pair
- key is the index
- value is the value 
- two ways to create dictionary
- my_dict = {}
- my_dict = dict()
- key value are separate colons
- key can only be any immutable data type: str, int, float, tuple, bool
- value can be anything!
- All information is stored like a dictionary, you supply the key and you get the value


```python
# Dictionaries
# student
#     - name
#     - email
#     - id
#     - major



# Most people use this
my_dict = {
    'name': 'john',
    'email': 'john@email.com',
    'id': 1234,
    'major': 'Engineering'
}

# Can also do this
my_dict = dict(
    name='john',
    email='john@email.com',
    id=1234,
    major='Engineering'
)


## accessing value
key = 'name'
my_dict['name']
my_dict[key]

# ## iterating over dictionaries

for value in my_dict.values():
    print(value)

for key in my_dict.keys():
    print(key)

## access both
for key, value in my_dict.items():
    print(f'key is {key} and value is {value}')

# ## test if dict contains a key

if key in my_dict:
    print(True)
else:
    print(False)

## test if dict contains a value
# default is testing in key
value = 'john'
if value in my_dict.values():
    print(True)
else:
    print(False)

# # dictionary methods
my_dict.clear() ## empty dict

my_dict.copy() ## copy dict -- unique objects in memory
# # check == and is; == is for values and is for memory

# # create default dictionary with initial value
new_student = {}.fromkeys(
    ['name', 'email', 'id', 'major'], 'missing')

my_dict = {}.fromkeys(range(5), 'iammissing')

# ## get
my_dict.get('name', None) # default
my_dict.get('name', False)
my_dict.get('name', 'defaultname')


# ## pop remove value using key
my_dict.pop('name')

# ## update -- way to append to dictionary
my_dict.update({'year':2010, 'ear': 2030})


# ## dictionary comprehension
# {___:___ for ___ in ___}

# my_dict = {}.fromkeys(range(5), 5)

# {key: key*value for key, value in my_dict.items()}
# {num: num*num for num in range(5)}

list1 = ['john', 'jane', 'doe']
list2 = [95, 99, 98]

{list1[i]: list2[i] for i in range(len(list1))}

dict(zip(list1,list2))


{num: ("even" if num % 2 == 0 else "odd") for num in range(1, 20)}

```