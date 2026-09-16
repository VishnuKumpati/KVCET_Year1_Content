# Sorting with a Key

Sorting so far has compared whole items. Numbers sorted as numbers, strings alphabetically, and mixed types not at all. That is often not the order wanted: words by length, records by one field, names ignoring case.

`sorted()` and `sort()` both take a `key` — a function that says what to sort **by**.

## How key Works

The key function is called once for each item. Whatever it returns is used for the comparison, and the items themselves are what come out:

```python
words = ["banana", "fig", "cherry", "date"]
print(sorted(words))
print(sorted(words, key=len))
```

**Output:**

```
['banana', 'cherry', 'date', 'fig']
['fig', 'date', 'banana', 'cherry']
```

The first is alphabetical. The second is by length: `fig` is 3, `date` is 4, and `banana` and `cherry` are both 6.

`len` is passed without brackets, the same rule as `map()` and `filter()`. `sorted()` needs the function, not the result of calling it.

Note what came back: the original words, not their lengths. The key decides the order and nothing else.

## Case-Insensitive Sorting

Sorting strings alphabetically puts every capital before every lowercase letter, which is rarely intended:

```python
fruits = ["banana", "Apple", "cherry", "Date"]
print(sorted(fruits))
print(sorted(fruits, key=str.lower))
```

**Output:**

```
['Apple', 'Date', 'banana', 'cherry']
['Apple', 'banana', 'cherry', 'Date']
```

`str.lower` is the `lower()` method, referred to without calling it. Each item is lowercased for comparison purposes only, so the order is alphabetical while the original capitalisation survives in the result.

This is the fix for the capitalisation trap, and it is worth applying by default to any list of names or titles.

## Sorting Records by a Field

A list of dictionaries cannot be sorted without a key, because dictionaries are not comparable with `<`. A lambda that picks out one field supplies the ordering:

```python
students = [
    {"name": "Anita", "mark": 78},
    {"name": "Ravi", "mark": 91},
    {"name": "Meera", "mark": 64},
]

by_mark = sorted(students, key=lambda student: student["mark"])
print([student["name"] for student in by_mark])
```

**Output:**

```
['Meera', 'Anita', 'Ravi']
```

This is the single most common use of `key`, and it is the one job with no comprehension equivalent. A comprehension can filter and transform; it cannot reorder.

Sorting by a computed value is the same, with the computation in the lambda:

```python
students = [
    {"name": "Anita", "marks": [78, 91, 64]},
    {"name": "Ravi", "marks": [88, 72, 95]},
    {"name": "Meera", "marks": [60, 85, 70]},
]

by_average = sorted(students, key=lambda s: sum(s["marks"]) / len(s["marks"]))
for student in by_average:
    print(f"{student['name']:<10}{sum(student['marks']) / len(student['marks']):>8.1f}")
```

**Output:**

```
Meera         71.7
Anita         77.7
Ravi          85.0
```

## reverse Alongside key

`reverse=True` works with a key, and reverses the order the key produced:

```python
students = [
    {"name": "Anita", "mark": 78},
    {"name": "Ravi", "mark": 91},
    {"name": "Meera", "mark": 64},
]

ranked = sorted(students, key=lambda student: student["mark"], reverse=True)
for position, student in enumerate(ranked, start=1):
    print(f"{position}. {student['name']:<10}{student['mark']:>4}")
```

**Output:**

```
1. Ravi        91
2. Anita       78
3. Meera       64
```

For numbers, negating inside the key does the same job:

```python
marks = [78, 91, 64]
print(sorted(marks, key=lambda mark: -mark))
```

**Output:**

```
[91, 78, 64]
```

That looks like a needless trick until two fields have to sort in opposite directions, which is what the next section covers. For a single field, `reverse=True` says what it means and is the better choice.

## Sorting by Several Things

A key can return a tuple, and tuples compare item by item: the first items decide, and the second are consulted only when the first are equal.

```python
students = [
    {"name": "Ravi", "course": "Maths", "mark": 91},
    {"name": "Anita", "course": "Maths", "mark": 78},
    {"name": "Meera", "course": "Physics", "mark": 64},
    {"name": "Sunil", "course": "Maths", "mark": 91},
]

ordered = sorted(students, key=lambda s: (s["course"], -s["mark"], s["name"]))
for student in ordered:
    print(f"{student['course']:<10}{student['mark']:>4}  {student['name']}")
```

**Output:**

```
Maths       91  Ravi
Maths       91  Sunil
Maths       78  Anita
Physics     64  Meera
```

Course ascending, then mark descending, then name ascending. The negation is what reverses the middle field on its own — `reverse=True` would have flipped all three.

## Stable Sorting

When two items have equal keys, Python keeps them in the order they were already in. This is called a **stable** sort.

```python
words = ["banana", "fig", "cherry", "date"]
print(sorted(words, key=len))
```

**Output:**

```
['fig', 'date', 'banana', 'cherry']
```

`banana` and `cherry` are both six letters, and `banana` came first in the original list, so it comes first here.

Stability is a guarantee, not an accident, and it is useful: sorting twice sorts by two fields. Sort by the less important field first, then by the more important one, and the first ordering survives inside each group of the second.

## The Key Is Called Once per Item

`sorted()` computes every key first, then sorts using those values. It does not call the key function each time it compares two items.

```python
def show_key(word):
    print("computing key for", word)
    return len(word)

print(sorted(["bb", "a", "ccc"], key=show_key))
```

**Output:**

```
computing key for bb
computing key for a
computing key for ccc
['a', 'bb', 'ccc']
```

Three items, three calls. Sorting three items takes more than three comparisons, so the keys are clearly being reused.

This matters when the key is expensive. A key that reads a file or does a long calculation runs once per item rather than once per comparison, which makes `key` the right place for such work — and a reason to prefer it over older approaches that compared items pairwise.

It also means the key function must be **pure**: given the same item it should return the same value, and it should not change anything. A key with side effects runs an unpredictable number of times relative to the rest of the program.

## min and max Take the Same Key

`min()` and `max()` accept `key` and interpret it identically:

```python
words = ["banana", "fig", "cherry"]
print(min(words, key=len))
print(max(words, key=len))
```

**Output:**

```
fig
banana
```

`banana` and `cherry` are both longest; `max()` returns the first one it found, consistent with stability.

On records, this finds the whole record rather than just the winning value:

```python
students = [
    {"name": "Anita", "mark": 78},
    {"name": "Ravi", "mark": 91},
    {"name": "Meera", "mark": 64},
]

best = max(students, key=lambda student: student["mark"])
print(best)
print(best["name"], "scored", best["mark"])
```

**Output:**

```
{'name': 'Ravi', 'mark': 91}
Ravi scored 91
```

`max(student["mark"] for student in students)` would give `91` and lose the name. `max(students, key=...)` gives the record, which is almost always what the question actually wanted.

## sort Takes It Too

Everything above applies to the `sort()` method, with the usual difference: it reorders the list in place and returns `None`.

```python
names = ["banana", "Apple", "cherry"]
names.sort(key=str.lower)
print(names)
```

**Output:**

```
['Apple', 'banana', 'cherry']
```

Use `sort(key=...)` when the list's own order should change, and `sorted(key=...)` when the original must survive.

## Further Reading

- **Official Python sorting guide** — https://docs.python.org/3/howto/sorting.html
- **Sorting with key functions** — https://realpython.com/python-sort/

A key function chooses what to sort by, works with `reverse`, extends to several fields through tuples, and serves `min()` and `max()` unchanged. Next, a way of producing values that does not build a list at all.
