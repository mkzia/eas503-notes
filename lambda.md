# Lambda, Filter, Map, and Sorted

## Lambda
lambda a function without a name;
- anonymous functions
- one line only
- single expression only
- stored in a variable
- When do you use it? 
  - Pass a function into another function as a parameter
- lambda variables (comma separated): expression
- lambda functions are used with other functions. Usually with map, filter, sort

## Filter
- Uses `function` to test the truthiness of each value in the sequence and returns a filtered list.
- `function` must return True or False

```python
filter(function, sequence)
```
## Map
- Uses `function` to transform the values in a sequence 
```python
map(function, sequence)
```

## Sorted
- Uses `function` to sort values
```python
sorted(sequence, key=function)
```