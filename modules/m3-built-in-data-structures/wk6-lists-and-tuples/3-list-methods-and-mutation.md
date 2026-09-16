# List Methods and Mutation

Assigning to `marks[1]` changed the list without creating a new one. That property has a name.

A value is **mutable** if it can be changed in place, and **immutable** if it cannot. A list is mutable. Every type met so far — `int`, `float`, `str`, `bool` — is immutable, so this is the first type whose contents can genuinely be altered.

The distinction matters more than it first appears, and the second half of this topic shows why. First, the methods that do the changing.

## Adding Items

Three methods add to a list, and they differ in where and how much.

`append()` adds one item at the end:

```python
items = ["a", "b"]
items.append("c")
print(items)
```

**Output:**

```
['a', 'b', 'c']
```

`insert()` adds one item at a chosen position, pushing everything after it along:

```python
items = ["a", "b", "c"]
items.insert(1, "x")
print(items)
```

**Output:**

```
['a', 'x', 'b', 'c']
```

`insert()` takes the index first and the value second. The new item ends up *at* that index.

`extend()` adds every item from another list:

```python
items = ["a", "b"]
items.extend(["c", "d"])
print(items)
```

**Output:**

```
['a', 'b', 'c', 'd']
```

The difference between `extend()` and `append()` is easy to miss and easy to see:

```python
items = ["a", "b"]
items.append(["c", "d"])
print(items)
print(len(items))
```

**Output:**

```
['a', 'b', ['c', 'd']]
3
```

`append()` added one item, and that item happens to be a list. `extend()` would have added two items. Use `extend()` to join lists and `append()` to add a single value.

## Removing Items

Three ways remove, and they differ in what you have to know.

`remove()` removes by **value**, taking out the first match:

```python
items = ["a", "b", "c", "b"]
items.remove("b")
print(items)
```

**Output:**

```
['a', 'c', 'b']
```

Only the first `"b"` went. A value that is not present is an error:

```python
items = ["a", "b"]
items.remove("z")
```

**Output:**

```
ValueError: list.remove(x): x not in list
```

`pop()` removes by **position**, and hands the removed value back:

```python
items = ["a", "b", "c"]
last = items.pop()
print(last)
print(items)
```

**Output:**

```
c
['a', 'b']
```

With no index, `pop()` takes the last item. With an index, it takes that one:

```python
items = ["a", "b", "c"]
first = items.pop(0)
print(first)
print(items)
```

**Output:**

```
a
['b', 'c']
```

`pop()` is the right choice when you need the value as well as its removal.

`del` is a statement rather than a method, and it deletes without returning anything:

```python
items = ["a", "b", "c"]
del items[1]
print(items)
```

**Output:**

```
['a', 'c']
```

`del` can also delete a slice, which removes several items at once:

```python
items = ["a", "b", "c", "d", "e"]
del items[1:3]
print(items)
```

**Output:**

```
['a', 'd', 'e']
```

`clear()` empties the list completely:

```python
items = ["a", "b", "c"]
items.clear()
print(items)
print(len(items))
```

**Output:**

```
[]
0
```

The list still exists after `clear()`. It simply has nothing in it.

## Finding and Counting

`index()` reports the position of the first matching value:

```python
names = ["Anita", "Ravi", "Meera"]
print(names.index("Ravi"))
```

**Output:**

```
1
```

Like `remove()`, it raises `ValueError` if the value is absent, so test with `in` first when the value might be missing.

`count()` reports how many times a value appears, and returns `0` rather than an error when it appears not at all:

```python
votes = ["yes", "no", "yes", "yes"]
print(votes.count("yes"))
print(votes.count("maybe"))
```

**Output:**

```
3
0
```

## Summary of the Methods

| Method | Does | Returns |
| --- | --- | --- |
| `append(value)` | adds one item at the end | nothing |
| `insert(index, value)` | adds one item at `index` | nothing |
| `extend(other)` | adds every item of `other` | nothing |
| `remove(value)` | removes the first match | nothing |
| `pop()` | removes the last item | the removed item |
| `pop(index)` | removes the item at `index` | the removed item |
| `clear()` | removes every item | nothing |
| `index(value)` | finds the first match | its position |
| `count(value)` | counts the matches | the count |

Every method in the top half changes the list and returns nothing. Only `pop()`, `index()` and `count()` give you a value back.

## Two Names, One List

Here is the consequence of mutability.

Assignment does not copy a list. It gives the same list a second name:

```python
first = [1, 2, 3]
second = first
second.append(4)
print(second)
print(first)
```

**Output:**

```
[1, 2, 3, 4]
[1, 2, 3, 4]
```

Only `second` was appended to, and `first` changed as well. It has to, because there is only one list:

```
 first  ─┐
         ├──▶  [1, 2, 3, 4]
 second ─┘
```

This never happened with numbers. `b = a` followed by `b = b + 1` leaves `a` alone, because `b + 1` builds a new number and rebinds the name. `second.append(4)` does not build anything new; it reaches into the existing list and changes it, and every name pointing at that list sees the change.

## is Against ==

Two questions can be asked about two lists, and they have different answers.

`==` asks whether the contents match. `is` asks whether they are the same object:

```python
first = [1, 2, 3]
second = [1, 2, 3]
print(first == second)
print(first is second)
```

**Output:**

```
True
False
```

Same contents, two separate lists. Changing one would not touch the other.

Now with an assignment instead:

```python
first = [1, 2, 3]
second = first
print(first == second)
print(first is second)
```

**Output:**

```
True
True
```

Same contents, and one single list under two names. Changing it through either name changes what the other sees.

Use `==` to compare values, which is almost always what you want. `is` answers a question about identity, and its one everyday use is `if value is None`.

## Copying a List Properly

To get a genuine second list, copy it. Three ways do the same job:

```python
first = [1, 2, 3]
second = first.copy()
third = first[:]
fourth = list(first)

second.append(4)
print(first)
print(second)
print(first is second)
```

**Output:**

```
[1, 2, 3]
[1, 2, 3, 4]
False
```

`first` is untouched, because `second` is now a different list that started with the same contents.

## Passing a List to a Function

The same rule applies when a list is passed into a function. The parameter becomes another name for the caller's list, so changes made inside are visible outside:

```python
def add_bonus(scores):
    scores.append(10)

marks = [78, 91]
add_bonus(marks)
print(marks)
```

**Output:**

```
[78, 91, 10]
```

The function returned nothing, and `marks` changed anyway. Nothing was copied on the way in.

This is useful when it is intended and a genuine bug when it is not. If a function should leave its argument alone, copy inside it and return the new list:

```python
def with_bonus(scores):
    updated = scores.copy()
    updated.append(10)
    return updated

marks = [78, 91]
result = with_bonus(marks)
print(marks)
print(result)
```

**Output:**

```
[78, 91]
[78, 91, 10]
```

Decide which of the two a function is doing, and let its name say so. A name like `add_bonus` suggests it changes something; a name like `with_bonus` suggests it hands back something new.

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Mutable and immutable objects** — https://realpython.com/python-mutable-vs-immutable-types/

You can now build a list, change it, and reason about who else sees those changes. Next, you will walk through a list one item at a time.
