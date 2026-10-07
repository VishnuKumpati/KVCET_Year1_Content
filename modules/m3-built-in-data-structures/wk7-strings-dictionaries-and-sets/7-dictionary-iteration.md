# Dictionary Iteration

A `for` loop over a list gives one element on each iteration. A dictionary holds two things in each pair, so the loop has to be told which of them it is working with.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72, "Meera": 95}
for name in marks:
    print(name)
```

**Output:**

```
Asha
Ravi
Meera
```

The loop gave the keys, not the values and not the pairs. This is the default, and it is the first thing to know about looping through a dictionary.

The pairs arrive in the order they were added, because that is the order a dictionary keeps them in.

## Looping Over the Keys

Writing `marks.keys()` does the same thing as writing `marks`.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
for name in marks.keys():
    print(name)
```

**Output:**

```
Asha
Ravi
```

Both forms give the keys. `marks.keys()` states it, which some programmers prefer for clarity.

The key can then be used to reach its value.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
for name in marks:
    print(name, marks[name])
```

**Output:**

```
Asha 87
Ravi 72
```

`marks[name]` looks up the value for whichever key the loop is on.

## Looping Over the Values

When the keys are not needed, `values()` gives the values directly.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72, "Meera": 95}
total = 0
for mark in marks.values():
    total = total + mark
print("Total:", total)
```

**Output:**

```
Total: 254
```

The names never appeared. Only the marks were needed, so only the marks were taken.

## Looping Over the Pairs

The `items()` method gives the key and the value together, and the `for` line takes two variables to receive them.

**Syntax**

```
for key, value in dictionary_name.items():
    statements
```

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
for name, mark in marks.items():
    print(name, mark)
```

**Output:**

```
Asha 87
Ravi 72
```

`name` received the key and `mark` received the value on every iteration.

`items()` gives each pair as a tuple, and two variables unpack the key and value separately.

**Example**

```python
marks = {"Asha": 87, "Ravi": 72}
for pair in marks.items():
    print(pair)
```

**Output:**

```
('Asha', 87)
('Ravi', 72)
```

Two variables are almost always what a program wants, because the key and the value are then ready to use separately.

## Choosing Which Form to Use

Three forms, and the choice depends on what the loop body needs.

| Form | Gives | Use when |
| --- | --- | --- |
| `for key in dictionary` | each key | only the keys are needed |
| `for value in dictionary.values()` | each value | only the values are needed |
| `for key, value in dictionary.items()` | each key and value | both are needed |

`for key in dictionary` followed by `dictionary[key]` also works and does the same job as `items()`. `items()` is shorter and does not look the value up a second time.

## Using a Condition Inside the Loop

An `if` statement inside the loop decides what happens for each pair.

**Example**

```python
marks = {"Asha": 87, "Ravi": 30, "Meera": 95}
for name, mark in marks.items():
    if mark >= 35:
        print(name, "passed")
    else:
        print(name, "failed")
```

**Output:**

```
Asha passed
Ravi failed
Meera passed
```

## Building a New Dictionary

An empty dictionary created before the loop collects the pairs that are wanted.

**Example**

```python
marks = {"Asha": 87, "Ravi": 30, "Meera": 95}
passed = {}

for name, mark in marks.items():
    if mark >= 35:
        passed[name] = mark

print(passed)
```

**Output:**

```
{'Asha': 87, 'Meera': 95}
```

`passed` is a separate dictionary. The original is unchanged, which is what makes this approach safe.

## Changing a Dictionary During a Loop

A dictionary cannot gain or lose pairs while a loop is going through it.

**Example**

```python
marks = {"Asha": 87, "Ravi": 30}
for name in marks:
    if marks[name] < 35:
        del marks[name]
```

**Output:**

```
RuntimeError: dictionary changed size during iteration
```

Python detects the change and stops. This differs from a list, where removing during a loop causes elements to be skipped silently.

The safe approach is to build a new dictionary containing only the pairs you want to keep.

**Example**

```python
marks = {"Asha": 87, "Ravi": 30}
kept = {}

for name, mark in marks.items():
    if mark >= 35:
        kept[name] = mark

print(kept)
```

**Output:**

```
{'Asha': 87}
```

Changing a value is allowed, because the number of pairs stays the same. Only adding or removing a pair raises the error.

## Looping in Sorted Order

A dictionary keeps its pairs in the order they were added. The `sorted()` function gives the keys in order instead.

**Example**

```python
marks = {"Ravi": 72, "Asha": 87, "Meera": 95}
for name in sorted(marks):
    print(name, marks[name])
```

**Output:**

```
Asha 87
Meera 95
Ravi 72
```

`sorted(marks)` returns a sorted list of the keys. The dictionary itself is unchanged.

## Looping Over an Empty Dictionary

A loop over an empty dictionary runs zero times and raises no error.

**Example**

```python
marks = {}
for name in marks:
    print(name)
print("Loop finished")
```

**Output:**

```
Loop finished
```

## Further Reading

- 📎 **Looping through dictionaries with worked examples** — https://www.programiz.com/python-programming/dictionary
- 📎 **Official Python guide to dictionaries** — https://docs.python.org/3/tutorial/datastructures.html

Lists, tuples and strings can contain duplicate values, and dictionary values can also repeat. Sometimes you need a collection where every value is unique.

Next, you will learn about sets.
