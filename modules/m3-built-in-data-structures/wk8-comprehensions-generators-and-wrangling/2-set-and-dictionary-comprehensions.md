# Set and Dictionary Comprehensions

A list comprehension uses square brackets, and the result is a list. Curly braces give a set or a dictionary instead.

```
[expression for variable in collection]        a list
{expression for variable in collection}        a set
{key: value for variable in collection}        a dictionary
```

The `for` part and the `if` part work the same way as in a list comprehension. What changes is what the comprehension builds: a set stores values, while a dictionary stores key-value pairs.

## Set Comprehensions

A set comprehension builds a set, so duplicate values are kept only once.

**Syntax**

```
{expression for variable in collection}
```

**Example**

This example takes a list of answers and builds a set of the languages, with each one capitalised.

```python
answers = ["tamil", "hindi", "tamil", "telugu"]
languages = {answer.title() for answer in answers}
print(sorted(languages))
print(len(languages))
```

**Output:**

```
['Hindi', 'Tamil', 'Telugu']
3
```

**Explanation:**

- `answer` takes each value from the list, one at a time
- `answer.title()` capitalises it, so `"tamil"` becomes `"Tamil"`
- the result is added to the set
- when `tamil` comes round the second time, `Tamil` is already there, so it is kept only once

Sets do not give us a particular order to rely on when displaying their values. `sorted()` puts the values in alphabetical order so we can see a predictable output.

**Example**

This example keeps only the passing scores, with the duplicates removed.

```python
scores = [87, 32, 72, 87, 25]
passed = {score for score in scores if score >= 35}
print(sorted(passed))
```

**Output:**

```
[72, 87]
```

**Explanation:**

- `score` takes each value from the list
- `score >= 35` decides whether it is kept, so `32` and `25` are skipped
- `87` passes the condition twice, and the set stores it once

## Dictionary Comprehensions

A dictionary comprehension builds pairs, so the expression has two parts separated by a colon.

**Syntax**

```
{key: value for variable in collection}
```

The part before the colon becomes the key. The part after it becomes the value.

**Example**

This example builds a dictionary in which each name is a key and the length of that name is its value.

```python
names = ["asha", "ravi", "meera"]
lengths = {name: len(name) for name in names}
print(lengths)
```

**Output:**

```
{'asha': 4, 'ravi': 4, 'meera': 5}
```

**Explanation:**

- `name` takes each value from the list
- `name` before the colon becomes the key
- `len(name)` after the colon becomes the value
- so `"asha"` gives the pair `'asha': 4`

## Building a Dictionary from a Dictionary

`items()` gives a key and a value on each pass, which the comprehension can use in both halves.

**Example**

This example builds a new dictionary with the same names and five added to every score.

```python
results = {"Asha": 87, "Ravi": 72}
bonus = {name: score + 5 for name, score in results.items()}
print(bonus)
```

**Output:**

```
{'Asha': 92, 'Ravi': 77}
```

**Explanation:**

- `items()` gives one `name` and one `score` on each pass
- `name` is kept as the key
- `score + 5` becomes the new value
- so `Asha: 87` becomes `Asha: 92`

**Example**

This example keeps only the pairs whose score is a pass.

```python
results = {"Asha": 87, "Ravi": 30, "Meera": 95}
passed = {name: score for name, score in results.items() if score >= 35}
print(passed)
```

**Output:**

```
{'Asha': 87, 'Meera': 95}
```

**Explanation:**

- `Asha` has 87, so `Asha: 87` is added
- `Ravi` has 30, so the pair is skipped
- `Meera` has 95, so `Meera: 95` is added

With curly braces, no colon means a set; a `key: value` pair means a dictionary.

## Further Reading

- 📎 **Set comprehensions with worked examples** — https://www.programiz.com/python-programming/set-comprehension
- 📎 **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html

So far, our comprehensions have used simple expressions such as `len(name)` and `score + 5`. Python also lets us write a small function directly where we need it, without giving it a name.

Next, you will learn about lambda functions.
