# map() and filter()

A lambda becomes useful when we pass it to another function. Python's built-in `map()` and `filter()` functions are common examples of this.

`map()` applies a function to every item in an iterable. `filter()` keeps the items for which a function returns `True`.

## The map() Function

In its simplest form, `map()` takes two arguments: a function and an iterable. It applies the function to each item and produces the results.

**Syntax**

```
map(function, iterable)
```

**Example**

```python
scores = [87, 72, 95]
bonus = map(lambda score: score + 5, scores)
print(list(bonus))
```

**Output:**

```
[92, 77, 100]
```

The lambda receives one score at a time. `87` gives `92`, `72` gives `77`, and `95` gives `100`.

The function does not have to be a lambda. Any function name works.

**Example**

```python
def add_bonus(score):
    return score + 5

scores = [87, 72, 95]
print(list(map(add_bonus, scores)))
```

**Output:**

```
[92, 77, 100]
```

`add_bonus` is written without brackets, because the function is being handed to `map()` rather than called.

## map() Returns a map Object

`map()` returns a map object, which is an iterator rather than a list. It produces each result when needed.

**Example**

```python
scores = [87, 72, 95]
bonus = map(lambda score: score + 5, scores)
print(type(bonus))
```

**Output:**

```
<class 'map'>
```

Use `list()` when you want to collect all the results into a list. You can also process the results one at a time using a `for` loop.

A map object is an iterator. Once all its results have been consumed, there are no results left to retrieve from it.

**Example**

```python
scores = [87, 72]
bonus = map(lambda score: score + 5, scores)
print(list(bonus))
print(list(bonus))
```

**Output:**

```
[92, 77]
[]
```

The first `list()` used up the results. The second found nothing left.

## The filter() Function

`filter()` also takes a function and an iterable. The function tests each item. `filter()` keeps the items for which the function returns `True` in these examples.

**Syntax**

```
filter(function, iterable)
```

**Example**

```python
scores = [87, 32, 72, 25, 95]
passed = filter(lambda score: score >= 35, scores)
print(list(passed))
```

**Output:**

```
[87, 72, 95]
```

The lambda returns `True` for `87`, `72` and `95`, so those are kept. It returns `False` for `32` and `25`, so those are dropped.

`filter()` never changes the values. It only decides which ones stay.

**Example**

```python
names = ["Asha", "Ravi", "Meera"]
print(list(filter(lambda name: len(name) > 4, names)))
```

**Output:**

```
['Meera']
```

Only `Meera` has more than four characters, and it comes back exactly as it was.

When nothing matches, the result is empty.

**Example**

```python
scores = [20, 30]
print(list(filter(lambda score: score >= 35, scores)))
```

**Output:**

```
[]
```

## The Difference Between map() and filter()

Both take a function and an iterable, and what the function returns is what separates them.

| `map()` | `filter()` |
| --- | --- |
| Applies a function to each item to produce a result. | Tests each item to decide whether to keep it. |
| Produces one result per input item. | Keeps only items whose test result is truthy. |
| With one input iterable, produces the same number of results as input items. | Produces the same number of items or fewer. |
| The results may differ from the original items. | Keeps the original items; it does not transform them. |

## Using Them Together

The result of `filter()` can be passed straight into `map()`.

**Example**

```python
scores = [87, 32, 72, 25, 95]

passed = filter(lambda score: score >= 35, scores)
bonus = map(lambda score: score + 5, passed)

print(list(bonus))
```

**Output:**

```
[92, 77, 100]
```

`filter()` removed `32` and `25` first. `map()` then added five to each of the three that remained.

## Comparing with a Comprehension

A list comprehension does the same work.

**Example**

```python
scores = [87, 72, 95]

print(list(map(lambda score: score + 5, scores)))
print([score + 5 for score in scores])
```

**Output:**

```
[92, 77, 100]
[92, 77, 100]
```

The comprehension needs no lambda and no `list()`, which is why it is the more common choice in Python for this kind of work.

`map()` and `filter()` are still worth knowing. They appear in existing code, and they are useful when the function already has a name and does not need a lambda at all.

## Further Reading

- 📎 **map() and filter() with worked examples** — https://www.programiz.com/python-programming/methods/built-in/map
- 📎 **Official Python reference for built-in functions** — https://docs.python.org/3/library/functions.html

`map()` and `filter()` produce results on demand instead of building a list immediately. This is the same iterator behaviour that makes them useful when processing data. Python also lets us create our own iterators using generators.

Next, you will learn about generators.
