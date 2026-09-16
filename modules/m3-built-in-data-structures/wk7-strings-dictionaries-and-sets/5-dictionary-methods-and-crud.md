# Dictionary Methods and CRUD

Four operations cover almost everything done to stored data: **create**, **read**, **update** and **delete**. The initials give the name **CRUD**, and the term is used throughout software, from a dictionary in memory to a database serving millions of records.

A dictionary supports all four directly.

## The Four Operations

| Operation | Syntax | On a missing key |
| --- | --- | --- |
| Create | `record[key] = value` | creates the pair |
| Read | `record[key]` | `KeyError` |
| Read | `record.get(key, default)` | returns the default |
| Update | `record[key] = value` | creates the pair |
| Delete | `del record[key]` | `KeyError` |
| Delete | `record.pop(key, default)` | returns the default |

Create and update share one syntax, which is the point worth holding on to. The same line does both, and which one happens depends only on whether the key already exists.

All four in sequence:

```python
record = {}

record["name"] = "Anita"
record["course"] = "Computer Science"
print(record)

print(record["name"])

record["course"] = "Mathematics"
print(record)

del record["course"]
print(record)
```

**Output:**

```
{'name': 'Anita', 'course': 'Computer Science'}
Anita
{'name': 'Anita', 'course': 'Mathematics'}
{'name': 'Anita'}
```

## keys, values and items

Three methods give access to the parts of a dictionary.

`keys()` gives the keys:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print(ages.keys())
```

**Output:**

```
dict_keys(['Anita', 'Ravi', 'Meera'])
```

`values()` gives the values:

```python
print(ages.values())
```

**Output:**

```
dict_values([21, 22, 20])
```

`items()` gives each pair as a tuple:

```python
print(ages.items())
```

**Output:**

```
dict_items([('Anita', 21), ('Ravi', 22), ('Meera', 20)])
```

Each returns a **view**, not a list. A view can be looped over and tested with `in`, and it is the natural thing to hand to a `for` loop. What it cannot do is be indexed:

```python
ages = {"Anita": 21, "Ravi": 22}
print(ages.keys()[0])
```

**Output:**

```
TypeError: 'dict_keys' object is not subscriptable
```

Convert it when a real list is wanted:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
names = list(ages.keys())
print(names)
print(names[0])
print(sorted(ages.values()))
```

**Output:**

```
['Anita', 'Ravi', 'Meera']
Anita
[20, 21, 22]
```

A view stays connected to the dictionary. Change the dictionary and the view reflects it:

```python
ages = {"Anita": 21}
names = ages.keys()
ages["Ravi"] = 22
print(names)
```

**Output:**

```
dict_keys(['Anita', 'Ravi'])
```

`names` was never reassigned. It is a window onto the dictionary, not a snapshot of it.

## in Tests Keys

`in` on a dictionary looks at the keys and nothing else:

```python
ages = {"Anita": 21, "Ravi": 22}
print("Anita" in ages)
print(21 in ages)
```

**Output:**

```
True
False
```

`21` is a value, and `in` did not look at the values. Say so explicitly when that is the question:

```python
print(21 in ages.values())
```

**Output:**

```
True
```

`"Anita" in ages` and `"Anita" in ages.keys()` mean the same thing. The shorter form is the usual one.

## update

`update()` merges one dictionary into another:

```python
defaults = {"theme": "light", "font": "serif"}
chosen = {"theme": "dark"}

defaults.update(chosen)
print(defaults)
```

**Output:**

```
{'theme': 'dark', 'font': 'serif'}
```

Keys that exist are overwritten, keys that do not are added. `"theme"` was replaced and `"font"` survived, which is exactly the behaviour wanted when user settings are laid over defaults.

The dictionary it is called on is changed. The one passed in is not:

```python
print(chosen)
```

**Output:**

```
{'theme': 'dark'}
```

`update()` also takes keyword arguments, which is shorter for a couple of literal values:

```python
record = {"name": "Anita"}
record.update(age=21, course="Computer Science")
print(record)
```

**Output:**

```
{'name': 'Anita', 'age': 21, 'course': 'Computer Science'}
```

## setdefault

`setdefault()` handles the read-or-create case: give back the value for a key, and if the key is absent, store a default first.

```python
record = {"name": "Anita"}

print(record.setdefault("name", "Unknown"))
print(record.setdefault("grade", "Not graded"))
print(record)
```

**Output:**

```
Anita
Not graded
{'name': 'Anita', 'grade': 'Not graded'}
```

The first call found `"name"` and returned its value, leaving the dictionary alone. The second did not find `"grade"`, so it added the pair and returned the new value.

That is the difference from `get()`. Both return a default for a missing key; only `setdefault()` writes it into the dictionary:

| Call | Returns | Changes the dictionary |
| --- | --- | --- |
| `record.get(key, default)` | the value, or `default` | never |
| `record.setdefault(key, default)` | the value, or `default` | when the key is missing |

Its real use is building a dictionary whose values are lists, where the list has to exist before anything can be appended to it:

```python
students = [
    ("Anita", "Computer Science"),
    ("Ravi", "Mathematics"),
    ("Meera", "Computer Science"),
]

by_course = {}
for name, course in students:
    by_course.setdefault(course, []).append(name)

print(by_course)
```

**Output:**

```
{'Computer Science': ['Anita', 'Meera'], 'Mathematics': ['Ravi']}
```

One line did the grouping. `setdefault(course, [])` returned the existing list for a course already seen, or created an empty one for a course seen for the first time, and `.append(name)` then added to whichever list came back. Without it, the same loop needs an `if` to check whether the key exists yet.

## clear and copy

`clear()` empties a dictionary, leaving it in place:

```python
ages = {"Anita": 21, "Ravi": 22}
ages.clear()
print(ages)
```

**Output:**

```
{}
```

`copy()` makes a genuine second dictionary:

```python
original = {"a": 1, "b": 2}
duplicate = original.copy()
duplicate["c"] = 3

print(original)
print(duplicate)
print(original is duplicate)
```

**Output:**

```
{'a': 1, 'b': 2}
{'a': 1, 'b': 2, 'c': 3}
False
```

The warning that came with list copying applies here unchanged. `duplicate = original` does not copy anything; it gives the same dictionary a second name, and a change through either name is visible through both.

There is a further limit worth knowing. `copy()` copies the pairs, not the values behind them. If a value is itself a list or dictionary, both copies point at that same inner object:

```python
original = {"marks": [78, 91]}
duplicate = original.copy()
duplicate["marks"].append(64)

print(original)
```

**Output:**

```
{'marks': [78, 91, 64]}
```

The outer dictionaries are separate, and the inner list is shared. This is called a **shallow** copy, and it is what `copy()` on a list does too. It is only a problem when the values are mutable, which is exactly the case in the nested structures met later.

## Further Reading

- **Official dictionary method reference** — https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
- **Dictionary methods with examples** — https://www.programiz.com/python-programming/methods/dictionary

Create, read, update and delete all have their syntax, and `keys()`, `values()` and `items()` open a dictionary up. Next, you will loop over one.
