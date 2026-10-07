# Sets

Lists, tuples and strings keep their values in order, and the same value can appear in them any number of times. Both of those behaviours are useful when the order matters or when repeated values need to be kept.

Some questions want neither. A class of sixty students is asked which language each speaks at home. Thirty answer Tamil, and listing every answer gives Tamil thirty times. The question was which languages are spoken, and the answer is Tamil once. The order of the languages does not matter either.

Python has a data type for exactly this.

> **A set is a collection of unique values.** A value can appear only once in a set. A set has no positions, so its values cannot be accessed by index.

**Example**

```python
answers = ["Tamil", "Hindi", "Tamil", "Telugu", "Hindi", "Tamil"]
languages = set(answers)
print(len(languages))
```

**Output:**

```
3
```

Six students answered, but only three unique languages were found. The repeated answers do not create additional values in the set, so Tamil and Hindi each appear only once.

## Syntax of a Set

A set is written with curly braces, and the values inside are separated by commas.

**Syntax**

```
variable = {value1, value2, value3}
```

**Example**

```python
subjects = {"Maths", "Science", "English"}
print(len(subjects))
print(type(subjects))
```

**Output:**

```
3
<class 'set'>
```

`set` is a built-in data type, like `list` and `dict`.

The braces are the same braces a dictionary uses. A dictionary holds pairs written with colons, and a set holds single values.

A value written twice is stored once.

**Example**

```python
marks = {87, 72, 87}
print(sorted(marks))
print(len(marks))
```

**Output:**

```
[72, 87]
2
```

No error is raised. The duplicate is simply dropped.

## Removing Duplicates from a List

The `set()` function builds a set from an existing list, keeping only one copy of each value.

**Example**

```python
marks = [87, 72, 87, 95, 72]
unique = set(marks)
print(len(unique))
```

**Output:**

```
3
```

This is one of the most common uses of a set.

## A Set Has No Order

A set does not keep its values in the order they were written.

**Example**

```python
numbers = {5, 1, 3}
print(sorted(numbers))
```

**Output:**

```
[1, 3, 5]
```

`sorted()` creates a sorted list from the set. Without `sorted()`, the values may appear in a different order.

Because there is no order, there are no positions either.

**Example**

```python
subjects = {"Maths", "Science"}
print(subjects[0])
```

**Output:**

```
TypeError: 'set' object is not subscriptable
```

There is no first value in a set, so indexing and slicing do not apply.

## Creating an Empty Set

Empty curly braces create a dictionary, not a set. The `set()` function creates an empty set.

**Example**

```python
empty = set()
print(empty)
print(len(empty))

not_a_set = {}
print(type(not_a_set))
```

**Output:**

```
set()
0
<class 'dict'>
```

An empty set prints as `set()` rather than as empty braces, because `{}` already means an empty dictionary.

## Adding and Removing Values

The `add()` method adds one value. The `remove()` method removes one.

**Syntax**

```
set_name.add(value)
set_name.remove(value)
```

**Example**

```python
subjects = {"Maths"}
subjects.add("Science")
print(len(subjects))

subjects.add("Maths")
print(len(subjects))
```

**Output:**

```
2
2
```

The second `add()` changed nothing, because `Maths` was already there.

`remove()` raises an error when the value is absent.

**Example**

```python
subjects = {"Maths"}
subjects.remove("History")
```

**Output:**

```
KeyError: 'History'
```

The `discard()` method removes a value and does nothing when it is absent.

**Example**

```python
subjects = {"Maths"}
subjects.discard("History")
print(subjects)
```

**Output:**

```
{'Maths'}
```

Use `remove()` when the value should be there, and `discard()` when it may not be.

## Values a Set Can Hold

A set can hold strings, numbers and tuples. A list cannot be a value in a set.

**Example**

```python
bad = {[1, 2]}
```

**Output:**

```
TypeError: unhashable type: 'list'
```

## Checking Whether a Value Is in a Set

The `in` operator tests whether a value is in the set.

**Example**

```python
students = {"Asha", "Ravi", "Meera"}
print("Meera" in students)
print("Kiran" in students)
```

**Output:**

```
True
False
```

Membership checking is one of the main reasons to choose a set over a list.

## Looping Through a Set

A `for` loop gives one value on each iteration. The order is not the order the values were written in.

**Example**

```python
subjects = {"Maths", "Science", "English"}
for subject in sorted(subjects):
    print(subject)
```

**Output:**

```
English
Maths
Science
```

`sorted()` was used so the output is in a predictable order. Without it, the three lines could appear in any order.

## Comparing Two Sets

Sets can be compared to find common or different values.

| Operator | Meaning |
| --- | --- |
| `\|` | Values in either or both sets |
| `&` | Values in both sets |
| `-` | Values only in the first set |
| `^` | Values in one set but not both |

**Example**

```python
science = {"Asha", "Ravi", "Meera"}
maths = {"Ravi", "Meera", "Kiran"}

print(sorted(science | maths))
print(sorted(science & maths))
print(sorted(science - maths))
print(sorted(science ^ maths))
```

**Output:**

```
['Asha', 'Kiran', 'Meera', 'Ravi']
['Meera', 'Ravi']
['Asha']
['Asha', 'Kiran']
```

- `|` combines both sets.
- `&` finds common values.
- `-` finds values only in the first set.
- `^` finds values that appear in only one set.

`sorted()` is used so the results appear in a fixed order, which is why the output shows square brackets.

## Choosing Between a List and a Set

Both store collections of values. The way you need to use those values determines which one to choose.

| List | Set |
| --- | --- |
| Keeps elements in order. | Has no order. |
| Allows duplicate values. | Keeps one copy of each value. |
| Elements are accessed by index. | Values cannot be accessed by index. |
| Suited to data where order and repeats matter. | Suited to membership checks and comparisons between groups. |

## Further Reading

- 📎 **Sets with worked examples** — https://www.programiz.com/python-programming/set
- 📎 **Official Python guide to sets** — https://docs.python.org/3/tutorial/datastructures.html

Every collection so far has held values directly. A value can also be a collection of its own, which is how a program stores a list inside a dictionary or a dictionary inside a list.

Next, you will learn about nested data structures.
