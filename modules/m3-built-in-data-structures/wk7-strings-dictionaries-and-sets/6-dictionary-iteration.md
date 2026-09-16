# Dictionary Iteration

A dictionary keeps its pairs in the order they were inserted, so looping over one is predictable. What changes between the forms below is which part of each pair you get.

## Looping Gives the Keys

A `for` loop over a dictionary takes one **key** per iteration:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
for name in ages:
    print(name)
```

**Output:**

```
Anita
Ravi
Meera
```

Not the values, and not the pairs. This catches people out, because looping over a list gives the items rather than their positions.

With the key in hand, the value is one lookup away:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
for name in ages:
    print(f"{name} is {ages[name]}")
```

**Output:**

```
Anita is 21
Ravi is 22
Meera is 20
```

`for name in ages` and `for name in ages.keys()` are the same loop. The shorter form is more common.

## Looping over items

When both parts are wanted, `items()` supplies them together:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
for name, age in ages.items():
    print(f"{name} is {age}")
```

**Output:**

```
Anita is 21
Ravi is 22
Meera is 20
```

Same output, no lookup inside the loop. `items()` produces a tuple for each pair, and the two names on the left unpack it — the same unpacking that `enumerate()` and `zip()` rely on.

Written without the unpacking, what is happening becomes visible:

```python
ages = {"Anita": 21, "Ravi": 22}
for pair in ages.items():
    print(pair)
```

**Output:**

```
('Anita', 21)
('Ravi', 22)
```

Each item is a two-item tuple, key first. `for name, age in ...` simply names the two parts as they arrive.

This is the form to reach for by default. It says plainly that both halves of the pair are in use.

## Looping over values

When the keys are irrelevant, `values()` gives the values alone:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
for age in ages.values():
    print(age)
```

**Output:**

```
21
22
20
```

The built-in functions that take a sequence work on a view directly, which often removes the loop entirely:

```python
ages = {"Anita": 21, "Ravi": 22, "Meera": 20}
print(sum(ages.values()))
print(max(ages.values()))
print(sum(ages.values()) / len(ages))
```

**Output:**

```
63
22
21.0
```

## Choosing the Form

| Loop | Gives | Use when |
| --- | --- | --- |
| `for key in record` | keys | only the keys matter |
| `for key, value in record.items()` | both | both matter |
| `for value in record.values()` | values | only the values matter |

Say what you need. A loop over `items()` that ignores the value, or a loop over keys that looks every value up, both misreport what the code is doing.

## Building a Dictionary in a Loop

Counting one thing needs a single variable. Counting many things at once needs a dictionary, and the pattern is the counter pattern with the counter looked up by key:

```python
sentence = "the cat sat on the mat the end"
words = sentence.split()

counts = {}
for word in words:
    counts[word] = counts.get(word, 0) + 1

print(counts)
```

**Output:**

```
{'the': 3, 'cat': 1, 'sat': 1, 'on': 1, 'mat': 1, 'end': 1}
```

The whole count is one line. `counts.get(word, 0)` returns the running total for a word already seen, or `0` for a word met for the first time, and the assignment stores the result back. No `if` is needed, because `get()` with a default covers both cases.

The same shape counts anything that can be reduced to a key. Grades, for instance:

```python
marks = [78, 91, 64, 88, 45]

grade_counts = {}
for mark in marks:
    if mark >= 85:
        grade = "A"
    elif mark >= 70:
        grade = "B"
    elif mark >= 50:
        grade = "C"
    else:
        grade = "F"
    grade_counts[grade] = grade_counts.get(grade, 0) + 1

print(grade_counts)
```

**Output:**

```
{'B': 1, 'A': 2, 'C': 1, 'F': 1}
```

The keys appear in the order each grade was first awarded. `78` produced the first `"B"`, `91` the first `"A"`, and so on.

Building a dictionary of totals rather than counts is the same pattern with a different thing added:

```python
sales = [("north", 120), ("south", 80), ("north", 45), ("east", 200)]

totals = {}
for region, amount in sales:
    totals[region] = totals.get(region, 0) + amount

print(totals)
```

**Output:**

```
{'north': 165, 'south': 80, 'east': 200}
```

## Do Not Resize While Looping

The rule about lists applies here and is enforced rather than merely advised:

```python
counts = {"a": 1, "b": 0, "c": 2}
for key in counts:
    if counts[key] == 0:
        del counts[key]
```

**Output:**

```
RuntimeError: dictionary changed size during iteration
```

Loop over a snapshot instead, which `list()` provides:

```python
counts = {"a": 1, "b": 0, "c": 2}
for key in list(counts.keys()):
    if counts[key] == 0:
        del counts[key]
print(counts)
```

**Output:**

```
{'a': 1, 'c': 2}
```

Changing a value in place is fine. Only adding or removing keys is the problem.

## Sorting by Key

A dictionary cannot be sorted in place — there is no `sort()` method on one. What you can do is produce a sorted order and use it.

`sorted()` on a dictionary sorts its keys and returns them as a list:

```python
ages = {"Ravi": 22, "Anita": 21, "Meera": 20}
print(sorted(ages))
```

**Output:**

```
['Anita', 'Meera', 'Ravi']
```

Loop over that to report in key order:

```python
ages = {"Ravi": 22, "Anita": 21, "Meera": 20}
for name in sorted(ages):
    print(f"{name}: {ages[name]}")
```

**Output:**

```
Anita: 21
Meera: 20
Ravi: 22
```

The dictionary itself is unchanged. Only the order of the report was chosen.

## Sorting by Value

Sorting by value takes one extra step, and it uses a fact about tuples: a list of tuples sorts by the first item of each, falling back to the second when the first ties.

So build pairs with the value first, then sort:

```python
ages = {"Ravi": 22, "Anita": 21, "Meera": 20}

pairs = []
for name, age in ages.items():
    pairs.append((age, name))

pairs.sort()
print(pairs)
```

**Output:**

```
[(20, 'Meera'), (21, 'Anita'), (22, 'Ravi')]
```

Unpack the pairs to report them:

```python
for age, name in pairs:
    print(f"{name}: {age}")
```

**Output:**

```
Meera: 20
Anita: 21
Ravi: 22
```

`sort(reverse=True)` gives the highest first, which is what a leaderboard needs:

```python
counts = {"the": 3, "cat": 1, "mat": 2}

pairs = []
for word, count in counts.items():
    pairs.append((count, word))

pairs.sort(reverse=True)
for count, word in pairs:
    print(f"{word}: {count}")
```

**Output:**

```
the: 3
mat: 2
cat: 1
```

If a sorted dictionary is wanted rather than a sorted report, build a new one from the sorted pairs. Insertion order is preserved, so the new dictionary keeps that order:

```python
ages = {"Ravi": 22, "Anita": 21, "Meera": 20}

pairs = []
for name, age in ages.items():
    pairs.append((age, name))
pairs.sort()

by_age = {}
for age, name in pairs:
    by_age[name] = age

print(by_age)
```

**Output:**

```
{'Meera': 20, 'Anita': 21, 'Ravi': 22}
```

## Further Reading

- **Official Python guide to looping techniques** — https://docs.python.org/3/tutorial/datastructures.html#looping-techniques
- **Iterating over dictionaries** — https://realpython.com/iterate-through-dictionary-python/

Looping over a dictionary reaches keys, values or both, and the counter pattern extends to counting many things at once. Next, you will meet a collection that holds no duplicates and no order at all.
