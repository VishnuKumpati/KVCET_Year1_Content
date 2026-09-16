# Generators and yield

A list comprehension builds every value and holds all of them at once. For a few hundred items that is invisible. For a large enough number it is the whole problem:

```python
import sys

squares = [number * number for number in range(100000)]
print(sys.getsizeof(squares), "bytes")
```

**Output:**

```
800984 bytes
```

Eight hundred kilobytes for a hundred thousand numbers, and every one of them computed before the first is used. A million items would take ten times that. A file with a hundred million lines would not fit at all.

Often none of it is necessary. Code that loops over the values once and discards them needs only one value at a time.

## Iterable and Iterator

Two words are used loosely in conversation and mean different things.

An **iterable** is anything a `for` loop can walk through: a list, a tuple, a string, a set, a dictionary, a `range`. It can be walked repeatedly, and it knows all its items.

An **iterator** is something that produces values one at a time and remembers where it has got to. It can be walked through **once**. `map()` and `filter()` return iterators, which is why their results empty after one pass.

`iter()` turns an iterable into an iterator, and `next()` asks an iterator for its next value:

```python
numbers = [1, 2, 3]
values = iter(numbers)

print(next(values))
print(next(values))
print(next(values))
```

**Output:**

```
1
2
3
```

Asking once more raises an error:

```python
print(next(values))
```

**Output:**

```
StopIteration
```

`StopIteration` is the signal that there is nothing left. It is not a bug — it is how an iterator says it has finished, and it is exactly what a `for` loop watches for. Every `for` loop you have ever written calls `iter()` on its sequence and then `next()` repeatedly until `StopIteration` arrives.

The list itself is untouched by all this. `numbers` can still be looped over again; it is `values`, the iterator over it, that is spent.

## A Generator Function

A **generator function** is written like an ordinary function, with `yield` where `return` would go:

```python
def countdown(start):
    while start > 0:
        yield start
        start = start - 1

for number in countdown(3):
    print(number)
```

**Output:**

```
3
2
1
```

No list was built. Each number was produced when the loop asked for it.

The presence of `yield` anywhere in the body is what makes it a generator function. Calling it does not run the body at all — it hands back a generator object:

```python
def countdown(start):
    while start > 0:
        yield start
        start = start - 1

counter = countdown(3)
print(type(counter))
```

**Output:**

```
<class 'generator'>
```

A generator is an iterator, so `next()` works on it and a `for` loop consumes it.

## What yield Does That return Does Not

`return` ends a function. Its local variables are discarded, and calling it again starts from the top.

`yield` **pauses** the function. It hands a value back and freezes everything — the local variables, the position in the loop, the whole state of the call. The next request resumes it from exactly that point.

Printing from inside makes the pausing visible:

```python
def steps():
    print("-- running to the first yield")
    yield 1
    print("-- resumed, running to the second yield")
    yield 2
    print("-- resumed, function ends")

generator = steps()
print("generator created")
print(next(generator))
print(next(generator))
```

**Output:**

```
generator created
-- running to the first yield
1
-- resumed, running to the second yield
2
```

`generator created` printed first. Nothing inside the function ran until `next()` was called, and the body then ran only as far as the first `yield`. The second `next()` picked up immediately after it.

One more request runs the rest of the body and raises:

```python
print(next(generator))
```

**Output:**

```
-- resumed, function ends
StopIteration
```

Reaching the end of the body is what raises `StopIteration`, so a `for` loop over `steps()` would simply end at that point.

| | `return` | `yield` |
| --- | --- | --- |
| Effect on the function | ends it | pauses it |
| Local variables | discarded | kept |
| Values per call | one | as many as asked for |
| Calling the function | runs the body | builds a generator |

A `return` inside a generator is still allowed, and it stops the generator early rather than producing a value.

## Generators Are Single-Use

A generator is an iterator, so the one-pass rule applies:

```python
def countdown(start):
    while start > 0:
        yield start
        start = start - 1

counter = countdown(3)
print(list(counter))
print(list(counter))
```

**Output:**

```
[3, 2, 1]
[]
```

The generator was exhausted by the first `list()`. To walk the values again, call the function again — `countdown(3)` produces a fresh generator each time.

This is the trade. A list can be read any number of times and costs memory proportional to its length. A generator costs almost nothing and can be read once.

## The Memory Difference

The same hundred thousand squares, as a generator function:

```python
import sys

def squares(limit):
    for number in range(limit):
        yield number * number

print(sys.getsizeof(squares(100000)), "bytes")
```

**Output:**

```
200 bytes
```

Two hundred bytes against eight hundred thousand, and the figure does not change with the limit. A generator holds a paused function, not values, so its size is the same whether it will produce ten items or ten million.

Summing them works identically either way, and the generator never holds more than one value at a time:

```python
def squares(limit):
    for number in range(limit):
        yield number * number

print(sum(squares(100000)))
```

**Output:**

```
333328333350000
```

## Values Without End

A generator can produce values forever, which no list can do. The loop inside it simply never finishes:

```python
def naturals():
    number = 1
    while True:
        yield number
        number = number + 1

for value in naturals():
    if value > 5:
        break
    print(value)
```

**Output:**

```
1
2
3
4
5
```

`while True` would be an infinite loop in an ordinary function. Here it is not, because the function only runs when a value is requested and spends the rest of its existence paused. The consumer decides when to stop.

The consumer must decide, though. `list(naturals())` would ask for values until the program ran out of memory, and there is nothing Python can do to prevent that.

A more useful endless generator produces a sequence with no natural limit:

```python
def fibonacci():
    current, following = 0, 1
    while True:
        yield current
        current, following = following, current + following

for number in fibonacci():
    if number > 100:
        break
    print(number, end=" ")
print()
```

**Output:**

```
0 1 1 2 3 5 8 13 21 34 55 89 
```

The state — two numbers — lives in the paused function between requests, and the swap in the last line is tuple packing and unpacking doing the work of a temporary variable.

## When to Use One

Use a generator when the values are consumed once, when there are many of them, or when there is no sensible limit on how many there might be. Reading a large file line by line, producing an endless sequence, or feeding a pipeline that ends in `sum()` or `max()` are all cases where holding the values would be waste.

Use a list when the values are needed more than once, when their number is known and modest, or when indexing, slicing, sorting or `len()` are required — none of which a generator supports.

## Further Reading

- **Official Python guide to generators** — https://docs.python.org/3/tutorial/classes.html#generators
- **Generators explained** — https://realpython.com/introduction-to-python-generators/

A generator function pauses at each `yield` and resumes on request, producing values one at a time without holding any of them. Next, the same idea in comprehension form.
