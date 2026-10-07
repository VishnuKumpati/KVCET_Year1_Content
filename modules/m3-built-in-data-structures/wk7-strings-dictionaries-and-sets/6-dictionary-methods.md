# Dictionary Methods

Square brackets let you get a value when you already know its key. But programs often need to do more: safely get a value when a key may be missing, work with all the keys or values, remove a pair, or combine dictionaries.

Dictionary methods provide these operations.

## The get() Method

The `get()` method returns the value for a key. If the key is missing, it returns `None` instead of raising an error.

**Syntax**

```
dictionary_name.get(key)
dictionary_name.get(key, default)
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks.get("Asha"))
print(marks.get("Kiran"))
```

**Output:**

```
87
None
```

The first call returned the value. The second returned `None` instead of raising an error.

A second argument sets what to return instead of `None`.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks.get("Kiran", 0))
```

**Output:**

```
0
```

`marks["Kiran"]` would raise a `KeyError` here. `get()` is used when a missing key is expected and should not stop the program.

## The keys() Method

The `keys()` method returns all the keys of the dictionary.

**Syntax**

```
dictionary_name.keys()
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks.keys())
```

**Output:**

```
dict_keys(['Asha', 'Ravi'])
```

You use `keys()` when you need to work with the dictionary's keys rather than look up one specific value.

## The values() Method

The `values()` method returns all the values of the dictionary.

**Syntax**

```
dictionary_name.values()
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks.values())
```

**Output:**

```
dict_values([87, 72])
```

You use `values()` when you need to work with the stored values without needing their keys. Values can repeat, because only keys are required to be unique.

## The items() Method

The `items()` method returns each key together with its value.

**Syntax**

```
dictionary_name.items()
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks.items())
```

**Output:**

```
dict_items([('Asha', 87), ('Ravi', 72)])
```

You use `items()` when you need both the key and its value together.

## The pop() Method

The `pop()` method removes a key and returns the value that was stored under it.

**Syntax**

```
dictionary_name.pop(key)
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
removed = marks.pop("Ravi")
print(removed)
print(marks)
```

**Output:**

```
72
{'Asha': 87}
```

`del` removes the pair and returns nothing. `pop()` removes the pair and returns the value.

## The update() Method

The `update()` method adds the pairs of another dictionary to this one.

**Syntax**

```
dictionary_name.update(other_dictionary)
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
marks.update({"Meera": 95, "Asha": 90})
print(marks)
```

**Output:**

```
{'Asha': 90, 'Ravi': 72, 'Meera': 95}
```

`Meera` was not present, so the pair was added. `Asha` was present, so the value was replaced. This is the same rule that applies to assigning a key, applied to every pair at once.

## Method Reference

| Method | Purpose | Changes dictionary |
| --- | --- | --- |
| `get(key, default)` | safely get a value | No |
| `keys()` | get all keys | No |
| `values()` | get all values | No |
| `items()` | get keys and values together | No |
| `pop(key)` | remove a pair and return its value | Yes |
| `update(other)` | add or replace multiple pairs | Yes |

## Further Reading

- 📎 **Dictionary methods with worked examples** — https://www.programiz.com/python-programming/methods/dictionary
- 📎 **Official Python reference for dictionaries** — https://docs.python.org/3/library/stdtypes.html#dict

These methods are most useful when a program needs to process each key, value, or key-value pair.

Next, you will learn how to loop through a dictionary and process them one at a time.
