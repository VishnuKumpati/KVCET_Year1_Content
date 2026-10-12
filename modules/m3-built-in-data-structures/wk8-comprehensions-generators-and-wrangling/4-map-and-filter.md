# map() and filter()

A `lambda` lets you write a small function without defining it separately using `def`. But writing a function is only part of the task — you also need a way to apply it to data.

Python provides two built-in functions for this: `map()` and `filter()`. Both functions can take a `lambda` directly as an argument, so you can define a small function and use it where needed without creating a separate function with `def`.

`map()` applies the function to every item and produces the results. `filter()` uses the function to test each item and keeps only those whose result is truthy.

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

Use `list()` to collect all the results into a list, or process them one at a time with a `for` loop. A map object produces results as they are requested. Once all results have been consumed, that same map object cannot produce them again. You can create a new map object to process the items again.

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

`filter()` also takes a function and an iterable. The function tests each item, and `filter()` keeps the items whose test result is truthy.

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
| Transforms each input item. | Tests each input item. |
| Produces one result per input item when using one iterable. | Keeps only items whose test result is truthy. |
| Results can differ from the original items. | Retains the original items without transforming them. |

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

For simple transformations like this, a list comprehension is often clearer because it avoids both the lambda and the `list()` conversion. `map()` and `filter()` are still useful when you already have a named function to apply.

## Further Reading

- 📎 **map() and filter() with worked examples** — https://www.programiz.com/python-programming/methods/built-in/map
- 📎 **Official Python reference for built-in functions** — https://docs.python.org/3/library/functions.html

`map()` and `filter()` pass each item to a function and use what it returns. `sorted()` can do the same, using the function's result to decide the order of the items.

Next, you will learn how to sort with a key.
