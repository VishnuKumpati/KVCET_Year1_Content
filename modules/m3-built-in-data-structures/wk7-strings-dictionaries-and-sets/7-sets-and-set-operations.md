# Sets and Set Operations

A list keeps order and allows duplicates. A dictionary looks values up by key. A **set** gives up both order and duplicates, and in exchange answers two questions very well: is this value present, and what do two collections have in common.

A set is an unordered collection of unique values.

## Creating a Set

Values in curly brackets, without the colons that make a dictionary:

```python
numbers = {3, 1, 2}
print(numbers)
print(len(numbers))
```

**Output:**

```
{1, 2, 3}
3
```

Duplicates are discarded as the set is built:

```python
numbers = {3, 1, 2, 3, 1}
print(numbers)
print(len(numbers))
```

**Output:**

```
{1, 2, 3}
3
```

Five values were written and three were stored. No error, no warning — a set simply cannot hold the same value twice.

A set has no order. Printing one shows the items in whatever arrangement the set happens to use internally, and that arrangement is not the insertion order and is not something to rely on. When output order matters, sort it:

```python
names = {"Meera", "Anita", "Ravi"}
print(sorted(names))
```

**Output:**

```
['Anita', 'Meera', 'Ravi']
```

`sorted()` returns a list, as it always does.

## The Empty Set

Empty curly brackets were already taken:

```python
print(type({}))
```

**Output:**

```
<class 'dict'>
```

An empty set needs the `set()` function:

```python
empty = set()
print(empty)
print(type(empty))
print(len(empty))
```

**Output:**

```
set()
<class 'set'>
0
```

Note that an empty set even *prints* as `set()`, because `{}` would be misread as a dictionary.

`set()` also builds a set from any sequence:

```python
print(set([1, 2, 2, 3]))
print(sorted(set("hello")))
```

**Output:**

```
{1, 2, 3}
['e', 'h', 'l', 'o']
```

A string is a sequence of characters, so `set("hello")` gives the distinct characters — one `l`, not two.

## Adding and Removing

`add()` adds one value:

```python
letters = {"a", "b"}
letters.add("c")
letters.add("a")
print(sorted(letters))
```

**Output:**

```
['a', 'b', 'c']
```

Adding a value that is already there does nothing, and is not an error. That is often the point: a set can be added to repeatedly without any check for duplicates.

`remove()` and `discard()` both take a value out, and differ on a value that is not there:

```python
letters = {"a", "b", "c"}
letters.remove("b")
print(sorted(letters))
letters.discard("z")
print(sorted(letters))
letters.remove("z")
```

**Output:**

```
['a', 'c']
['a', 'c']
KeyError: 'z'
```

`discard()` is silent about a missing value. `remove()` raises `KeyError`. Choose `remove()` when the value's absence would mean something has gone wrong, and `discard()` when it is a normal case.

`pop()` removes and returns an arbitrary item, and `clear()` empties the set. Because there is no order, there is no "last" item for `pop()` to take, which makes it much less useful here than on a list.

## Membership

`in` is what a set is for:

```python
allowed = {"admin", "editor", "viewer"}
print("editor" in allowed)
print("guest" in allowed)
```

**Output:**

```
True
False
```

A set finds a value the way a dictionary finds a key: it computes a number from the value and goes straight to where that value would be. It does not compare against each item in turn. A list has to, which means checking membership in a list of a million items takes a million times as long as checking a list of one, while checking a set stays about as fast either way.

If a program does many membership tests against a fixed collection, store that collection as a set.

The same rule as dictionary keys applies to what a set can hold. Items must be immutable, so strings, numbers and tuples are fine and lists are not:

```python
bad = {[1, 2]}
```

**Output:**

```
TypeError: unhashable type: 'list'
```

## No Indexing

A set has no positions, so it cannot be indexed or sliced:

```python
numbers = {1, 2, 3}
print(numbers[0])
```

**Output:**

```
TypeError: 'set' object is not subscriptable
```

There is no first item to ask for. Looping works, and the order it produces is not guaranteed:

```python
numbers = {1, 2, 3}
for number in sorted(numbers):
    print(number)
```

**Output:**

```
1
2
3
```

Sort it when the order of the output matters.

## Removing Duplicates from a List

The commonest everyday use of a set is not set theory at all. Converting a list to a set drops the duplicates, and converting back gives a list again:

```python
marks = [78, 91, 78, 64, 91, 78]
unique = list(set(marks))
print(sorted(unique))
print(len(marks), "values,", len(unique), "distinct")
```

**Output:**

```
[64, 78, 91]
6 values, 3 distinct
```

The round trip loses the original order, because a set has none to keep. When the order must survive, `dict.fromkeys()` does the same job using a dictionary's insertion order:

```python
marks = [78, 91, 78, 64, 91, 78]
print(list(dict.fromkeys(marks)))
```

**Output:**

```
[78, 91, 64]
```

If only the count of distinct values is wanted, no conversion back is needed:

```python
words = "the cat sat on the mat the end".split()
print(len(words), "words,", len(set(words)), "distinct")
```

**Output:**

```
8 words, 6 distinct
```

## The Four Set Operations

Four operations combine two sets. Each has an operator and a named method.

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)
print(a & b)
print(a - b)
print(a ^ b)
```

**Output:**

```
{1, 2, 3, 4, 5, 6}
{3, 4}
{1, 2}
{1, 2, 5, 6}
```

| Operation | Operator | Method | Gives |
| --- | --- | --- | --- |
| Union | `a \| b` | `a.union(b)` | everything in either |
| Intersection | `a & b` | `a.intersection(b)` | only what is in both |
| Difference | `a - b` | `a.difference(b)` | in `a` but not in `b` |
| Symmetric difference | `a ^ b` | `a.symmetric_difference(b)` | in one but not both |

Union and intersection give the same answer whichever way round they are written. Difference does not: `a - b` gave `{1, 2}` and `b - a` gives `{5, 6}`.

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
print(b - a)
```

**Output:**

```
{5, 6}
```

Note what `&`, `|` and `^` are doing here. Those three symbols were bitwise operators on whole numbers, and they now carry a second meaning on sets. The meanings line up: bitwise AND keeps the bits set in both operands, and `&` on sets keeps the items in both sets.

The methods do the same work as the operators, with one practical difference. A method accepts any sequence, while an operator requires both sides to be sets:

```python
a = {1, 2, 3}
print(a.union([4, 5]))
print(a | [4, 5])
```

**Output:**

```
{1, 2, 3, 4, 5}
TypeError: unsupported operand type(s) for |: 'set' and 'list'
```

Use the methods when one side is a list, and the operators when both sides are sets.

These operations answer real questions directly. Which students take both courses, which take either, which take one but not the other:

```python
maths = {"Anita", "Ravi", "Sunil"}
physics = {"Ravi", "Meera", "Sunil"}

print("Both:", sorted(maths & physics))
print("Either:", sorted(maths | physics))
print("Maths only:", sorted(maths - physics))
print("Exactly one:", sorted(maths ^ physics))
```

**Output:**

```
Both: ['Ravi', 'Sunil']
Either: ['Anita', 'Meera', 'Ravi', 'Sunil']
Maths only: ['Anita']
Exactly one: ['Anita', 'Meera']
```

Doing the same with lists takes a loop and a membership test for each question.

## Subset and Superset

A set is a **subset** of another when every one of its items is in that other set. The other is then a **superset** of it:

```python
small = {1, 2}
big = {1, 2, 3, 4}

print(small.issubset(big))
print(big.issuperset(small))
print(big.issubset(small))
```

**Output:**

```
True
True
False
```

`<=` and `>=` do the same tests:

```python
print(small <= big)
print(big >= small)
```

**Output:**

```
True
True
```

A set is a subset of itself, so `big <= big` is `True`.

`isdisjoint()` tests the opposite relationship — no items in common at all:

```python
morning = {"Anita", "Ravi"}
evening = {"Meera", "Sunil"}
print(morning.isdisjoint(evening))
```

**Output:**

```
True
```

These tests answer permission and eligibility questions in one line. Whether a user's roles include everything a task requires is `required.issubset(user_roles)`.

## Choosing the Type

| | List | Tuple | Dictionary | Set |
| --- | --- | --- | --- | --- |
| Ordered | yes | yes | by insertion | no |
| Duplicates | yes | yes | one value per key | no |
| Indexed | yes | yes | by key | no |
| Mutable | yes | no | yes | yes |
| Written with | `[ ]` | `( )` | `{key: value}` | `{ }` or `set()` |

Order and duplicates are the deciding questions. If both matter, use a list. If neither does, and the work is membership testing or comparing collections, use a set.

## Further Reading

- **Official Python guide to sets** — https://docs.python.org/3/tutorial/datastructures.html#sets
- **Sets with worked examples** — https://www.programiz.com/python-programming/set

A set holds unique values with no order, tests membership quickly, and compares collections in a single operation. Next, you will combine all four types into structures that hold real records.
