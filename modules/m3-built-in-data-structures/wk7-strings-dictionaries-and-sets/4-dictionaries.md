# Dictionaries

Storing people's ages with two lists works, just about:

```python
names = ["Anita", "Ravi", "Meera"]
ages = [21, 22, 20]

position = names.index("Ravi")
print(ages[position])
```

**Output:**

```
22
```

Every lookup goes through a position. The two lists have to stay the same length and in the same order forever, and one item removed from one of them and not the other quietly corrupts every answer afterwards.

The real problem is that a list finds things **by position**, and the position here is meaningless. What is wanted is a lookup **by name**.

A **dictionary** does that. It stores pairs, and looks a value up by the name attached to it.

## Keys and Values

Each pair is a **key** and a **value**, written `key: value`, inside curly brackets:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print(ages)
```

**Output:**

```
{'Anita': 21, 'Ravi': 22, 'Meera': 20}
```

The key is what you look something up by. The value is what you get back. Here the keys are names and the values are ages, and the connection between them is stored in the structure itself rather than being implied by matching positions in two lists.

Longer dictionaries are usually written one pair to a line:

```python
student = {
    "name": "Anita",
    "age": 21,
    "course": "Computer Science",
    "average": 77.67,
}
```

This shape — keys naming the fields of one thing — is how a single record is normally represented.

The empty dictionary uses empty braces:

```python
counts = {}
print(counts)
print(len(counts))
```

**Output:**

```
{}
0
```

`len()` reports the number of pairs, not the number of keys plus values:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print(len(ages))
```

**Output:**

```
3
```

## Reading a Value

Square brackets take a key instead of a position:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print(ages["Ravi"])
```

**Output:**

```
22
```

No `index()` call, no second list, and no assumption about where `"Ravi"` sits.

Keys are exact. Case, spacing and type all have to match:

```python
ages = {"Anita": 21}
print(ages["anita"])
```

**Output:**

```
KeyError: 'anita'
```

## KeyError and get

A key that is not present is an error:

```python
ages = {"Anita": 21, "Ravi": 22}
print(ages["Sunil"])
```

**Output:**

```
KeyError: 'Sunil'
```

`KeyError` is to a dictionary what `IndexError` is to a list, and it is just as common. There are two ways to avoid it.

Test first with `in`, which checks the keys:

```python
ages = {"Anita": 21, "Ravi": 22}
if "Sunil" in ages:
    print(ages["Sunil"])
else:
    print("No record for Sunil")
```

**Output:**

```
No record for Sunil
```

Or use `get()`, which returns `None` instead of raising:

```python
ages = {"Anita": 21, "Ravi": 22}
print(ages.get("Ravi"))
print(ages.get("Sunil"))
```

**Output:**

```
22
None
```

A second argument sets what to return instead of `None`:

```python
ages = {"Anita": 21, "Ravi": 22}
print(ages.get("Sunil", 0))
print(ages.get("Sunil", "unknown"))
```

**Output:**

```
0
unknown
```

`get()` with a sensible default is often the cleanest form, because it removes the `if` entirely:

```python
counts = {"yes": 5}
counts["yes"] = counts.get("yes", 0) + 1
counts["no"] = counts.get("no", 0) + 1
print(counts)
```

**Output:**

```
{'yes': 6, 'no': 1}
```

Both lines are identical, and one updated an existing count while the other created a new one. `get("no", 0)` supplied the starting value for a key that did not yet exist.

Use `[]` when a missing key means a bug worth stopping for, and `get()` when it is a normal case with a sensible answer.

## Adding and Updating

Assignment adds a pair:

```python
ages = {"Anita": 21, "Ravi": 22}
ages["Meera"] = 20
print(ages)
```

**Output:**

```
{'Anita': 21, 'Ravi': 22, 'Meera': 20}
```

Assignment to an existing key replaces its value:

```python
ages["Anita"] = 22
print(ages)
```

**Output:**

```
{'Anita': 22, 'Ravi': 22, 'Meera': 20}
```

The syntax is the same in both cases. Whether a pair is created or changed depends only on whether the key was already there — there is no separate "add" operation, and no error for either.

That differs sharply from a list, where assigning to `items[5]` on a three-item list is an `IndexError`. A dictionary has no fixed size to run past.

A key appears once. Assigning to it again replaces the value; it does not store a second copy.

New keys are added at the end, and a dictionary keeps its pairs in the order they were inserted. Printing or looping over one gives that order back.

## Deleting

`del` removes a pair:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
del ages["Ravi"]
print(ages)
```

**Output:**

```
{'Anita': 21, 'Meera': 20}
```

`pop()` removes it and hands the value back:

```python
ages = {"Anita": 21, "Ravi": 22}
removed = ages.pop("Ravi")
print(removed)
print(ages)
```

**Output:**

```
22
{'Anita': 21}
```

Both raise `KeyError` on a missing key, but `pop()` takes a default in the same way `get()` does:

```python
ages = {"Anita": 21}
print(ages.pop("Sunil", "not found"))
print(ages)
```

**Output:**

```
not found
{'Anita': 21}
```

Note that `pop()` on a list takes a position and `pop()` on a dictionary takes a key. Both remove and return; they differ in how the thing to remove is identified.

## What Can Be a Key

Values can be anything at all. Keys cannot.

A key must be **immutable**. Strings, numbers, booleans and tuples work:

```python
mixed = {
    "name": "Anita",
    7: "seven",
    3.5: "three point five",
    True: "yes",
    (3, 5): "a coordinate",
}
print(mixed[7])
print(mixed[(3, 5)])
```

**Output:**

```
seven
a coordinate
```

Lists and dictionaries cannot be keys:

```python
bad = {["Anita", "Ravi"]: 21}
```

**Output:**

```
TypeError: unhashable type: 'list'
```

The reason is how a dictionary finds things. It computes a number from the key's contents and uses that to go straight to the value, which is why a lookup is fast no matter how many pairs are stored. If the key's contents could change afterwards, the computed number would no longer match and the value would become unreachable. Forbidding mutable keys prevents that.

A tuple of immutable values is fine as a key, which makes tuples the way to key something by a pair — a coordinate, or a year and month together.

In practice almost every key is a string.

## Dictionaries Are Mutable

A dictionary is mutable, so everything true of list mutation is true here. Two names can refer to one dictionary:

```python
first = {"a": 1}
second = first
second["b"] = 2
print(first)
print(first is second)
```

**Output:**

```
{'a': 1, 'b': 2}
True
```

And a function can change its caller's dictionary:

```python
def add_grade(record):
    record["grade"] = "A"

student = {"name": "Anita"}
add_grade(student)
print(student)
```

**Output:**

```
{'name': 'Anita', 'grade': 'A'}
```

The same care applies, and so does the same remedy: copy when the original must survive.

## List Against Dictionary

| | List | Dictionary |
| --- | --- | --- |
| Finds by | position | key |
| Written with | `[ ]` | `{ }` |
| Positions or keys | `0` upwards, automatic | chosen by you |
| Duplicates | allowed | one value per key |
| Missing lookup | `IndexError` | `KeyError` |
| Mutable | yes | yes |

Use a list when the items are many of the same kind of thing and order is what matters. Use a dictionary when each value has a name, and that name is how you will want to find it.

## Further Reading

- **Official Python guide to dictionaries** — https://docs.python.org/3/tutorial/datastructures.html#dictionaries
- **Dictionaries with worked examples** — https://www.programiz.com/python-programming/dictionary

A dictionary stores pairs and finds a value by its key. Next, you will work through the full set of operations on one.
