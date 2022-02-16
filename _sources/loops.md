# Loops

## Repeating code using loops

```
for <ele> in <sequence>:
    <body>
```

- The loop index variable `ele` takes on each successive value in the sequence, and the statements in the body of the loop are executed once for each value.
- For loops have a limitation -- you have to know how many times you are looping -- it is a definite loop. The number of iterations is determined when the loop starts. If you do not know how many times you will be looping, use a while loop, which is an indefinite loop that will continue to loop until its condition is no longer true.

```
while <condition>
    <body>
```

- Loop is controlled using `break` and `continue`. 


```python
range(start:stop:step) # [start:step:stop)
```


```python
values = [4, 10, 3, 8, -6]
for i in range(len(values)):
    print(i)
```


```python
values = [4, 10, 3, 8, -6]
for i in range(len(values)):
    print(i, values[i])
```

```python
values = [4, 10, 3, 8, -6]
for i in range(len(values)):
    print(i, values[i])
```

```python
values = [4, 10, 3, 8, -6]
for index, value in enumerate(values):
    print(index, value)
```



```python
values = [4, 10, 3, 8, -6]
for i in range(len(values)):
    values[i] = values[i] * 2
```

```python
metals = ['Li', 'Na', 'K']
weights = [6.941, 22.98976928, 39.0983]
for i in range(len(metals)):
    print(metals[i], weights[i])
```

```python
metals = ['Li', 'Na', 'K']
weights = [6.941, 22.98976928, 39.0983]
for metal, weight in zip(metals, weights):
    print(metal, weight)
```


```python
elements = [['Li', 'Na', 'K'], ['F', 'Cl', 'Br']]
for inner_list in elements:
    for item in inner_list:
        print(item)
```

```python
info = [['Isaac Newton', 1643, 1727],
    ['Charles Darwin', 1809, 1882],
    ['Alan Turing', 1912, 1954, 'alan@bletchley.uk']]
for item in info:
    print(len(item))
```

- Do for loop exercises
- Do modify loop exercises
- Do while loop exercises

## File Handling

```python
with open(filename, 'r') as file:
    for line in file:
        if not line.strip(): # used for skipping empty lines!
            continue
        # do something with line

```
