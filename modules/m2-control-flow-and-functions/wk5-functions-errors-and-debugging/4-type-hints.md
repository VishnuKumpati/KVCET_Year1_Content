# Type Hints

Look at this function definition:

```python
def add(first, second):
    return first + second
```

Nothing in that definition tells a reader what kind of values `first` and `second` are expected to receive. If they are numbers, `+` performs addition. If they are strings, `+` joins them together. The function definition does not tell us which behaviour was intended.

> **A type hint is an annotation written in a function definition, stating the data type a parameter or a return value is expected to have.** Python does not enforce a hint while the program runs. It exists for people reading the code, for code editors, and for separate type-checking tools.

## Syntax of a Type Hint

A parameter hint is written after the parameter name, separated by a colon:

```
def function_name(parameter: type):
    statements
```

In a real function, `parameter` and `type` are replaced with an actual name and an actual type:

```python
def greet(name: str):
    print("Welcome", name)

greet("Asha")
```

**Output:**

```
Welcome Asha
```

The output is unchanged. `name: str` added no behaviour. It states that `name` is expected to be a string.

The return hint is written after the brackets, using an arrow `->`, before the colon:

```
def function_name(parameter: type) -> type:
    statements
```

Written out with both kinds of hint in place:

```python
def add(first: int, second: int) -> int:
    return first + second

print(add(10, 5))
```

**Output:**

```
15
```

That one line now says three things. Both parameters are expected to be integers, and the function gives back an integer.

## Reading a Hinted Definition

Take the definition apart:

```
def add(first: int, second: int) -> int:
```

- `first` and `second` are the parameters
- `int` after each colon is the type that parameter is expected to receive
- `int` after the arrow is the type the function is expected to return

Read it as a sentence. `add` takes two integers and gives back an integer.

## Hints for the Basic Types

The four types already known are written by their Python names:

| Type | Hint | Example parameter |
| --- | --- | --- |
| integer | `int` | `marks: int` |
| floating-point | `float` | `average: float` |
| string | `str` | `name: str` |
| Boolean | `bool` | `passed: bool` |

A function can mix them freely:

```python
def has_passed(marks: int) -> bool:
    return marks >= 35

print(has_passed(87))
print(has_passed(20))
```

**Output:**

```
True
False
```

The hint `-> bool` says the function gives back `True` or `False`. A reader knows that before reading a single line of the body.

The parameter types and the return type do not have to match:

```python
def average(total: float, count: int) -> float:
    return total / count

print(average(240.0, 3))
```

**Output:**

```
80.0
```

## Hints on Functions That Return Nothing

A function that does not return a useful value can be annotated with `-> None`. If the function reaches the end without returning a value, Python returns `None`.

```python
def greet(name: str) -> None:
    print("Welcome", name)

greet("Asha")
```

**Output:**

```
Welcome Asha
```

`-> None` tells a reader that the function is not expected to return a useful value.

## Hints with Default Values

A parameter can carry a hint and a default value together. The type hint comes first, followed by the default value:

```
message: str = "Welcome"
```

That parameter inside a full definition:

```python
def greet(name: str, message: str = "Welcome") -> None:
    print(message, name)

greet("Asha")
greet("Ravi", "Good morning")
```

**Output:**

```
Welcome Asha
Good morning Ravi
```

## Hints Are Not Enforced

Python does not check type hints. The program runs whether the arguments match them or not:

```python
def add(first: int, second: int) -> int:
    return first + second

print(add("Py", "thon"))
```

**Output:**

```
Python
```

The hints said `int`. Two strings were passed, and Python joined them without complaint. No error was raised, because a hint is a note and not a rule.

A type hint also does not convert a value to that type:

```python
def show_name(name: str):
    print(name)

show_name(123)
```

**Output:**

```
123
```

Python does not automatically turn `123` into `"123"`. The hint only describes what the function is expected to receive.

## Reasons for Writing Type Hints

Since Python ignores them, hints exist for people and tools.

A reader understands the function from its first line, without reading the body or running it.

A code editor uses them. Given `name: str`, the editor can offer the string methods and warn when a number is passed instead.

Separate checking tools read them and report mismatches before the program is ever run. For example:

```python
def add(first: int, second: int) -> int:
    return first + second

print(add("10", "20"))
```

**Output:**

```
1020
```

Python ran the call and produced a result, because the hints are ignored. A type-checking tool reading the same code would report that `"10"` is a string while `first` is expected to be an `int`.

Documentation is written with them. Every function signature in the official Python documentation carries hints, which is how the documentation states what a function takes and returns.

## Further Reading

- **Type hints explained** — https://realpython.com/python-type-checking/
- **Official Python reference for annotations** — https://docs.python.org/3/library/typing.html

A function can now state what it takes and what it gives back. Inside the body, it also creates names of its own, and those names cannot be reached from outside.

Next, you will learn the rules that decide where a name in a program can be used.
