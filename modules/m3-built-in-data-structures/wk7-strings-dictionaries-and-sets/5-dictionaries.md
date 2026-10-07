# Dictionaries

Strings, lists and tuples so far have found a value by its position. `marks[0]` is the first mark, and `name[0]` is the first character.

Storing the marks of three students in a list means keeping their names in a second list, in the same order.

```python
names = ["Asha", "Ravi", "Meera"]
marks = [87, 72, 95]
```

To get Ravi's mark, you must first know that Ravi is at position 1. The list of marks holds no names, so the two lists must stay in the same order throughout the program.

A dictionary is designed for this situation.

> **A dictionary is a collection of key-value pairs.** The key is a name chosen by the programmer, and the value is the data stored under that name. A value is accessed by its key rather than by a position.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks["Asha"])
print(marks["Ravi"])
```

**Output:**

```
87
72
```

One collection holds both the names and the marks. `marks["Ravi"]` returns Ravi's mark, and the answer does not depend on where Ravi is stored.

The key is what makes a dictionary different from everything before it, and every rule in this topic is a rule about keys.

## Syntax of a Dictionary

Curly braces hold the pairs. Each pair is a key, a colon, then its value, and the pairs are separated by commas.

**Syntax**

```
variable = {key1: value1, key2: value2}
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72, "Meera": 95}
print(marks)
print(len(marks))
print(type(marks))
```

**Output:**

```
{'Asha': 87, 'Ravi': 72, 'Meera': 95}
3
<class 'dict'>
```

`len()` returns `3`, the number of pairs. `dict` is a built-in data type, like `list` and `str`.

A dictionary keeps its pairs in the order they were added, which is why the output above matches the order they were written in.

Several pairs read more easily on separate lines.

```python
marks = {
    "Asha": 87,
    "Ravi": 72,
    "Meera": 95
}
```

## Accessing a Value

The key is written in square brackets after the dictionary name.

**Syntax**

```
dictionary_name[key]
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks["Asha"])
```

**Output:**

```
87
```

The brackets are the same as those used for list indexing. A list takes a position, and a dictionary takes a key.

## Every Key Must Be Unique

A dictionary cannot hold the same key twice. When a key is written twice, only the last value assigned to it is kept.

**Example**

```python
marks = {"Asha": 87, "Asha": 95}
print(marks)
```

**Output:**

```
{'Asha': 95}
```

The dictionary holds one pair. A key identifies one value, which makes the lookup reliable.

## Keys Must Be Immutable

Dictionary keys must be immutable, which means they cannot be changed after they are created. Strings, numbers and tuples can be keys, but lists cannot.

**Example**

```python
mixed = {"name": "Asha", 7: "seven", (1, 2): "pair"}
print(mixed)
```

**Output:**

```
{'name': 'Asha', 7: 'seven', (1, 2): 'pair'}
```

A list used as a key raises an error.

**Example**

```python
bad = {[1, 2]: "pair"}
```

**Output:**

```
TypeError: unhashable type: 'list'
```

In practice, keys are nearly always strings because names make a dictionary easy to read.

## Adding and Updating Values

Assigning to a key does one of two things, and the key decides which.

**Syntax**

```
dictionary_name[key] = value
```

**Example**

```python
marks = {"Asha": 87}

marks["Ravi"] = 72
print(marks)

marks["Asha"] = 90
print(marks)
```

**Output:**

```
{'Asha': 87, 'Ravi': 72}
{'Asha': 90, 'Ravi': 72}
```

The same statement added a pair the first time and replaced a value the second. `Ravi` was absent, so the pair was created. `Asha` was present, so its value was overwritten.

Python does not warn in either case. Assigning to an existing key replaces its current value.

## KeyError

Reading a key that does not exist raises a `KeyError`.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
print(marks["Kiran"])
```

**Output:**

```
KeyError: 'Kiran'
```

`KeyError` names the key that was missing. It is the dictionary equivalent of the `IndexError` raised by a position outside a list.

A key must match exactly, including its capital letters.

**Example**

```python
marks = {"Asha": 87}
print(marks["asha"])
```

**Output:**

```
KeyError: 'asha'
```

`"asha"` and `"Asha"` are different strings, so they are different keys. This matters when a key comes from typed input, where the capitals cannot be relied on.

The `in` operator checks for a key before it is used.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
name = "Kiran"

if name in marks:
    print(marks[name])
else:
    print(name, "has no marks recorded")
```

**Output:**

```
Kiran has no marks recorded
```

`in` checks the keys only. Searching for `87` returns `False`, even though that value is stored in the dictionary.

## Removal Takes the Whole Pair

The `del` statement removes the key and its value together.

**Syntax**

```
del dictionary_name[key]
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
del marks["Ravi"]
print(marks)
```

**Output:**

```
{'Asha': 87}
```

A value cannot be removed on its own. The key and the value exist only as a pair.

## Values Can Be Any Data Type

The restrictions discussed so far apply to keys. Values can be any Python data type, including a list.

**Example**

```python
student = {"name": "Asha", "class": 7, "marks": [87, 72], "passed": True}
print(student["name"])
print(student["marks"])
```

**Output:**

```
Asha
[87, 72]
```

A dictionary can also describe one item by storing several named pieces of information about it.

`student["marks"]` returns the list, which can be used like any other list.

## Creating an Empty Dictionary

Empty curly braces create a dictionary with no pairs.

**Example**

```python
marks = {}
print(len(marks))

marks["Asha"] = 87
print(marks)
```

**Output:**

```
0
{'Asha': 87}
```

A dictionary can be changed after it is created, so an empty one is a normal starting point. Pairs are added, replaced and removed as the program runs.

## Further Reading

- 📎 **Dictionaries with worked examples** — https://www.programiz.com/python-programming/dictionary
- 📎 **Official Python guide to dictionaries** — https://docs.python.org/3/tutorial/datastructures.html

Reading one value at a time covers only part of what a program does with a dictionary. Getting a missing key without an error, pulling out all the keys or all the values, and merging two dictionaries each need a method of their own.

Next, you will learn the methods a dictionary provides.
