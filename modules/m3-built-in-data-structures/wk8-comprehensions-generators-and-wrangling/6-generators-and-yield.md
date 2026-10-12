# Generators and yield

A function can return a list containing several values at once. A generator produces values one at a time, as they are requested.

> **A generator is an object that provides values one at a time when requested.**
>
> A generator function uses the `yield` keyword to produce values one at a time. When a value is requested, the function runs until the next `yield`, provides that value, and pauses. It continues from where it stopped when another value is requested.

## Producing Values One at a Time

A function can already return many values by putting them in a list.

```python
def get_scores():
    print("Preparing scores")
    return [87, 72, 95]

scores = get_scores()

for score in scores:
    print(score)
```

**Output:**

```
Preparing scores
87
72
95
```

`Preparing scores` printed first. The function built the complete list and returned it before the loop processed a single value.

For three scores, the list uses little memory. For a million scores, Python must create and store all one million list elements before the loop processes the first score.

A generator can produce these scores one at a time without building a separate list to hold all the results.

## The yield Keyword

Here is the same function with `yield` in place of `return`.

```python
def get_scores():
    print("Preparing first score")
    yield 87

    print("Preparing second score")
    yield 72

    print("Preparing third score")
    yield 95
```

`return` sends back a result and ends the function. `yield` provides a value and pauses the function's execution. When another value is requested, the function continues from the statement after `yield`.

This function produces nothing yet. The next section shows why.

## Calling a Generator Function

A normal function starts running when it is called. A generator function instead returns a generator object without running its body.

```python
result = get_scores()
print("Generator created")
```

**Output:**

```
Generator created
```

`Preparing first score` did not appear. The call created a **generator object**, which is stored in `result`, and the function has not started running.

## Requesting Values with next()

The `next()` function requests one value from a generator object.

```python
result = get_scores()

print(next(result))
print(next(result))
print(next(result))
```

**Output:**

```
Preparing first score
87
Preparing second score
72
Preparing third score
95
```

The first `next()` starts the generator and runs it until the first `yield`. Each later call resumes the generator from where it paused and runs until the next `yield`. The `yield` statement provides a value to the generator's caller. Here, `next()` returns that value to `print()` for display. The generator then pauses until another value is requested.

Once no values remain, `next()` raises `StopIteration`.

The generator remembers its local variables and where it paused, so each request continues from that point rather than starting over.

## Using a Generator in a Loop

Calling `next()` by hand shows the mechanism. In practice, a `for` loop requests the values for you.

```python
def add_bonus(scores):
    for score in scores:
        yield score + 5

for score in add_bonus([87, 72, 95]):
    print(score)
```

**Output:**

```
92
77
100
```

The generator takes each score from the input list, adds five, and yields the result. The outer loop requests each result as needed, so the generator does not create a separate list to store all the bonus scores.

When all the values are wanted together, `list()` collects them.

```python
print(list(add_bonus([87, 72, 95])))
```

**Output:**

```
[92, 77, 100]
```

This stores all the generated values in memory, so you no longer save memory by avoiding a list of results.

## When the Values Run Out

A `for` loop stops because the generator eventually has nothing left to produce. Calling `next()` at that point raises `StopIteration`.

```python
def get_scores():
    yield 87

result = get_scores()
print(next(result))

try:
    print(next(result))
except StopIteration:
    print("No more scores")
```

**Output:**

```
87
No more scores
```

A `for` loop handles `StopIteration` for you, which is why the earlier loops ended without an error.

## Reusing a Generator

A generator object can be consumed until it is exhausted. Once that happens, it cannot be restarted.

```python
def get_scores():
    yield 87
    yield 72

result = get_scores()

print(list(result))
print(list(result))
```

**Output:**

```
[87, 72]
[]
```

The first `list()` collected both scores and exhausted the generator. The second found none remaining.

To get the values again, call the generator function again. This creates a new generator object, starting from the beginning.

```python
result = get_scores()
print(list(result))
```

**Output:**

```
[87, 72]
```

A list, in contrast, can be read any number of times.

## Generators and Lists

| List | Generator |
| --- | --- |
| Stores all its elements in memory. | Produces values as they are requested. |
| Can be reused, indexed, and measured with `len()`. | Is consumed as values are requested; does not directly support indexing or `len()`. |
| Useful when you need to access stored values repeatedly. | Useful when you can process values one at a time. |

A generator is not automatically better than a list. Choose a generator when producing values on demand is useful; choose a list when you need to store and revisit the complete collection.

## Further Reading

- 📎 **Generators with worked examples** — https://www.programiz.com/python-programming/generator
- 📎 **Official Python guide to generators** — https://docs.python.org/3/tutorial/classes.html#generators

Python also provides a shorter way to create a generator without defining a separate generator function. This is called a generator expression.

Next, you will learn about generator expressions.
