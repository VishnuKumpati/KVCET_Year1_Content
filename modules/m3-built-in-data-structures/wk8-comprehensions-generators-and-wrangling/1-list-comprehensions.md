# List Comprehensions

Building a new list from an existing collection often follows the same pattern: an empty list, a loop, and `append()`.

```python
names = ["asha", "ravi", "meera"]

titled = []
for name in names:
    titled.append(name.title())

print(titled)
```

**Output:**

```
['Asha', 'Ravi', 'Meera']
```

For many tasks like this, three parts stay the same: you create an empty list, loop through the items, and append the result. The part that changes from task to task is the expression, such as `name.title()`.

Python has a shorter form. You write the part you choose, and Python handles the rest.

```python
names = ["asha", "ravi", "meera"]
titled = [name.title() for name in names]
print(titled)
```

**Output:**

```
['Asha', 'Ravi', 'Meera']
```

This is a **list comprehension**. It builds a new list by going through a collection and putting a value into the new list for each item.

## Syntax of a List Comprehension

```
[expression for variable in collection]
```

The `for` part of the loop is still there, with the variable and collection in the same order.

```
titled = [ name.title()   for name in names ]
             ↑                    ↑
      what to put in        where the values come from
```

Read it aloud and it says what it does: for each name in names, store `name.title()`.

## Transforming Every Item

The expression decides what value is added to the new list.

**Example**

This example builds a list of the squares of the numbers.

```python
numbers = [1, 2, 3, 4]
squares = [number * number for number in numbers]
print(squares)
```

**Output:**

```
[1, 4, 9, 16]
```

**Explanation:**

- `number` takes each value from the list, one at a time
- `number * number` works out the square
- the result is added to the new list, so `3` gives `9`

## Filtering Values

An `if` at the end of the comprehension keeps only the values that satisfy it.

**Syntax**

```
[expression for variable in collection if condition]
```

**Example**

This example keeps only the scores that are a pass.

```python
scores = [87, 32, 72, 25, 95]
passed = [score for score in scores if score >= 35]
print(passed)
```

**Output:**

```
[87, 72, 95]
```

**Explanation:**

- `score` takes each value from the list
- `score >= 35` decides whether it is kept
- `32` and `25` fail the condition, so they are skipped
- the expression is just `score`, so the values that pass are stored unchanged

## Filtering and Transforming Together

The expression and the condition do two different jobs, so both can be used at once.

**Example**

This example keeps the passing scores and adds a bonus of five to each one.

```python
scores = [87, 32, 72, 25, 95]
bonus = [score + 5 for score in scores if score >= 35]
print(bonus)
```

**Output:**

```
[92, 77, 100]
```

**Explanation:**

- `score >= 35` selects the values first, so `32` and `25` never reach the expression
- `score + 5` is worked out for each selected value
- so `87` gives `92`, and `72` gives `77`

## Choosing a Result

Sometimes every value should produce a result, but the result depends on a condition. The `if/else` expression comes before the `for`.

**Syntax**

```
[value_if_true if condition else value_if_false for variable in collection]
```

**Example**

This example turns each score into the word `Pass` or `Fail`.

```python
scores = [87, 32, 72]
status = ["Pass" if score >= 35 else "Fail" for score in scores]
print(status)
```

**Output:**

```
['Pass', 'Fail', 'Pass']
```

**Explanation:**

- `score` takes each value from the list
- the condition decides which of the two words is stored
- `87` passes, so `"Pass"` is stored
- `32` fails, so `"Fail"` is stored

Three scores went in and three results came out. Nothing was removed, because this form gives a result for every value.

These two forms do different jobs:

`if` after the `for` filters items.

`if/else` before the `for` chooses the result for each item.

## Using a Comprehension with a Dictionary

`items()` gives a key and value on each pass, so we can unpack them into two variables.

**Example**

This example builds a list of the names whose score is a pass.

```python
results = {"Asha": 87, "Ravi": 30}
passed = [name for name, score in results.items() if score >= 35]
print(passed)
```

**Output:**

```
['Asha']
```

**Explanation:**

- `items()` gives one `name` and one `score` on each pass
- `score >= 35` decides whether the name is kept
- `Asha` has 87, so `Asha` is added
- `Ravi` has 30, so he is skipped
- the expression is `name`, so only the names appear in the result

## When Not to Use a Comprehension

Use a list comprehension when you are building a new list and the logic remains easy to read. If the loop has several steps or becomes difficult to understand, use a normal `for` loop instead.

## Further Reading

- 📎 **List comprehensions with worked examples** — https://www.programiz.com/python-programming/list-comprehension
- 📎 **Official Python guide to list comprehensions** — https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions

The same idea can also be used to build sets and dictionaries.

Next, you will learn set and dictionary comprehensions.