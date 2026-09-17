# Sorting Basics

Putting a list in order is common enough that Python provides two ways to do it. They produce the same order and differ in what they leave behind, and choosing the wrong one is a frequent source of confusion.

## sort

`sort()` is a list method. It rearranges the list in place:

```python
marks = [78, 91, 64]
marks.sort()
print(marks)
```

**Output:**

```
[64, 78, 91]
```

The list is the same list, with its items rearranged. Nothing new was created, and the original order is gone.

Because it changes the list rather than producing one, `sort()` returns nothing:

```python
marks = [78, 91, 64]
result = marks.sort()
print(result)
```

**Output:**

```
None
```

This is the same trap as `append()`, and it is worth stating plainly: `marks = marks.sort()` throws the list away and leaves `marks` holding `None`. Call `sort()` on its own line.

## sorted

`sorted()` is a built-in function. It leaves the original alone and returns a new list:

```python
marks = [78, 91, 64]
ordered = sorted(marks)
print(ordered)
print(marks)
```

**Output:**

```
[64, 78, 91]
[78, 91, 64]
```

Two lists now exist: the original in its original order, and a sorted copy.

`sorted()` accepts any sequence, not just a list, and always hands back a list:

```python
point = (3, 1, 2)
print(sorted(point))
```

**Output:**

```
[1, 2, 3]
```

That is also the only way to sort a tuple's values, since a tuple is immutable and has no `sort()` method of its own.

## Choosing Between Them

| | `items.sort()` | `sorted(items)` |
| --- | --- | --- |
| What it is | a list method | a built-in function |
| The original list | rearranged | untouched |
| What it returns | `None` | a new sorted list |
| Works on | lists only | any sequence |

Use `sort()` when the list's own order is what you want changed and nothing needs the old order. Use `sorted()` when the original must survive — because something else uses it, because it arrived as a function argument, or because you want the sorted version under a different name.

When in doubt, `sorted()` is the safer choice. It cannot destroy anything.

## Sorting Numbers and Strings

Numbers sort smallest first:

```python
numbers = [10, 2, 33, 4]
print(sorted(numbers))
```

**Output:**

```
[2, 4, 10, 33]
```

Note that `2` comes before `10`. These are numbers being compared as numbers, not as text.

Strings sort alphabetically:

```python
names = ["Meera", "Anita", "Ravi"]
print(sorted(names))
```

**Output:**

```
['Anita', 'Meera', 'Ravi']
```

With one catch. Comparison uses character codes, and every capital letter has a lower code than every lowercase letter, so capitals sort first:

```python
fruits = ["banana", "Apple", "cherry", "Date"]
print(sorted(fruits))
```

**Output:**

```
['Apple', 'Date', 'banana', 'cherry']
```

That is correct by character code and wrong by any reasonable reading of "alphabetical". It catches people out whenever real names or titles are sorted with inconsistent capitalisation.

## Reversing the Order

Both take a `reverse` setting, which sorts largest first:

```python
marks = [78, 91, 64]
print(sorted(marks, reverse=True))

marks.sort(reverse=True)
print(marks)
```

**Output:**

```
[91, 78, 64]
[91, 78, 64]
```

`reverse=True` sorts and then presents the result in descending order. It does not sort first and flip afterwards as a separate step, and for ordinary use the distinction does not matter.

## reverse Against reverse=True

`reverse()` is a separate method, and it does not sort at all. It flips the list into the opposite of its current order, whatever that order happens to be:

```python
names = ["Anita", "Ravi", "Meera"]
names.reverse()
print(names)
```

**Output:**

```
['Meera', 'Ravi', 'Anita']
```

That is the original order backwards, not reverse alphabetical order. Compare the two on the same list:

```python
names = ["Anita", "Ravi", "Meera"]
print(sorted(names, reverse=True))

names.reverse()
print(names)
```

**Output:**

```
['Ravi', 'Meera', 'Anita']
['Meera', 'Ravi', 'Anita']
```

Different results, because they answer different questions. `reverse=True` means "sorted, descending". `reverse()` means "flip what is there".

Like `sort()`, `reverse()` changes the list in place and returns `None`. The slice `names[::-1]` does the same flip as a new list, leaving the original alone.

## Mixed Types

Sorting compares items with `<`, so the items have to be comparable. A list mixing numbers and strings is not:

```python
values = [3, "apple", 1]
print(sorted(values))
```

**Output:**

```
TypeError: '<' not supported between instances of 'str' and 'int'
```

Python will not guess whether `3` belongs before or after `"apple"`, and refuses rather than inventing an answer.

Integers and floats do sort together, because they are both numbers and comparing them is well defined:

```python
values = [3, 1.5, 2]
print(sorted(values))
```

**Output:**

```
[1.5, 2, 3]
```

Booleans sort with numbers too, since `False` counts as `0` and `True` as `1`. That is rarely what anyone intended, and a list that needs sorting is usually a sign the items should all be the same type.

## Further Reading

- **Official Python sorting guide** — https://docs.python.org/3/howto/sorting.html
- **sort and sorted compared** — https://www.programiz.com/python-programming/methods/list/sort

A list can now be created, indexed, sliced, changed, walked and ordered, and a tuple offers the same sequence behaviour with its contents locked. These two types cover data that is found by position. What they cannot do is find a value by name, which is where the next set of types begins.
