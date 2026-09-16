# Decorators

`@property`, `@abstractmethod`, `@classmethod` and `@staticmethod` have all appeared without explanation. Each is a **decorator**: something that takes a function and returns a modified version of it.

The `@` line is a shorthand, and understanding what it stands for makes all four unmysterious.

## Functions Are Objects

The idea rests on a fact about Python: a function is an ordinary value. It can be assigned to a name, passed as an argument, and returned from another function.

A function assigned to a second name is the same function:

```python
def shout(text):
    return text.upper()

yell = shout
print(yell("hi"))
print(shout.__name__)
```

**Output:**

```
HI
shout
```

No brackets on `shout` in the assignment. `shout` is the function; `shout()` calls it.

Functions also carry attributes, such as `__name__` and `__doc__`. That will matter shortly.

A function can be defined inside another and returned:

```python
def make_greeter(greeting):
    def greet(name):
        print(f"{greeting}, {name}")
    return greet

hello = make_greeter("Hello")
namaste = make_greeter("Namaste")

hello("Anita")
namaste("Ravi")
```

**Output:**

```
Hello, Anita
Namaste, Ravi
```

Two different functions came out of one definition. Each remembers the `greeting` it was built with, even though `make_greeter` finished long ago. An inner function keeping hold of its enclosing function's variables is called a **closure**, and it is what makes decorators work.

## Writing a Decorator

A decorator takes a function and returns a replacement:

```python
def announce(func):
    def wrapper(*args, **kwargs):
        print(f"-- calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"-- finished {func.__name__}")
        return result
    return wrapper

def greet(name):
    print(f"Hello, {name}")

greet = announce(greet)
greet("Anita")
```

**Output:**

```
-- calling greet
Hello, Anita
-- finished greet
```

Reading it from the inside:

- `announce` receives the original function as `func`.
- `wrapper` is a new function that does something, calls `func`, and does something else.
- `announce` returns `wrapper` without calling it.
- `greet = announce(greet)` points the name `greet` at the wrapper.

The original function is untouched. The *name* now refers to something that wraps it.

`*args, **kwargs` in the wrapper is what lets one decorator work on any function, whatever arguments it takes. They are collected on the way in and passed straight through. Returning `result` matters just as much — a wrapper that forgets it turns every decorated function into one that returns `None`.

## The @ Syntax

`greet = announce(greet)` immediately after a definition is the whole pattern, and `@` says it more briefly:

```python
def announce(func):
    def wrapper(*args, **kwargs):
        print(f"-- calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"-- finished {func.__name__}")
        return result
    return wrapper

@announce
def greet(name):
    print(f"Hello, {name}")

greet("Anita")
```

**Output:**

```
-- calling greet
Hello, Anita
-- finished greet
```

`@announce` above `def greet` means exactly `greet = announce(greet)`. There is nothing else to it.

The advantage is placement. The decoration appears at the definition, where a reader sees it, rather than on a separate line further down that is easy to miss.

## functools.wraps

The wrapper replaces the function, including its identity:

```python
print(greet.__name__)
```

**Output:**

```
wrapper
```

Every decorated function in a program now reports itself as `wrapper`, which ruins tracebacks, `help()` and any code that inspects functions. The docstring is lost too.

`functools.wraps` copies the original's identity onto the wrapper:

```python
import functools

def announce(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@announce
def greet(name):
    """Greet someone."""
    print(f"Hello, {name}")

print(greet.__name__)
print(greet.__doc__)
```

**Output:**

```
greet
Greet someone.
```

`@functools.wraps(func)` is itself a decorator, applied to the wrapper. Use it in every decorator you write — it costs one line and there is no reason to omit it.

## Decorators That Take Arguments

A decorator taking arguments needs one more layer: a function that takes the arguments and returns the decorator.

```python
import functools

def repeat(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for count in range(times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}")

greet("Anita")
```

**Output:**

```
Hello, Anita
Hello, Anita
Hello, Anita
```

`@repeat(3)` calls `repeat(3)` first, which returns `decorator`, and *that* is applied to `greet`. The three levels are the cost of the argument, and the pattern is always the same shape.

## The Built-in Decorators

The four met earlier are now readable.

**`@property`** turns a method into an attribute-style read, as covered under encapsulation.

**`@abstractmethod`** marks a method that subclasses must implement.

**`@classmethod`** makes the first parameter the class rather than the instance, which is how alternative constructors are written.

**`@staticmethod`** removes the first parameter entirely. The method takes neither `self` nor `cls` — it is a plain function that lives inside the class for organisational reasons:

```python
class Pizza:
    size_names = {"S": "small", "L": "large"}

    def __init__(self, size):
        self.size = size

    def describe(self):
        return f"A {Pizza.size_names[self.size]} pizza"

    @classmethod
    def large(cls):
        return cls("L")

    @staticmethod
    def is_valid_size(size):
        return size in Pizza.size_names

print(Pizza("S").describe())
print(Pizza.large().describe())
print(Pizza.is_valid_size("L"))
print(Pizza.is_valid_size("XL"))
```

**Output:**

```
A small pizza
A large pizza
True
False
```

The three kinds of method, side by side:

| Decorator | First parameter | Needs an object | Typical use |
| --- | --- | --- | --- |
| none | `self` | yes | work on one object |
| `@classmethod` | `cls` | no | alternative constructors |
| `@staticmethod` | none | no | a related helper with no state |

`is_valid_size` could be a module-level function. It is a static method because it belongs conceptually to `Pizza`, and `Pizza.is_valid_size("L")` says so.

## What Decorators Are For

The recurring use is behaviour that surrounds many functions without belonging to any of them: timing, logging, caching, retrying, checking permissions, validating arguments. Written inline, that code is repeated in every function and tangles with what the function is actually for. As a decorator it is written once and applied by name.

The standard library ships several. `functools.lru_cache` remembers a function's results so repeated calls with the same arguments return instantly:

```python
import functools

@functools.lru_cache
def slow_square(number):
    print(f"computing {number}")
    return number * number

print(slow_square(4))
print(slow_square(4))
```

**Output:**

```
computing 4
16
16
```

The second call printed nothing, because the function never ran.

Decorators in web frameworks attach URLs to functions, and in test frameworks mark tests and supply fixtures. Recognising the pattern is what makes that code readable.

## Further Reading

- **Official reference for `functools`** — https://docs.python.org/3/library/functools.html
- **Decorators explained in depth** — https://realpython.com/primer-on-python-decorators/

A decorator takes a function and returns a wrapped version, and `@name` is shorthand for reassigning the name to the result. Next, a pair of dunder methods that wrap a block of code instead of a function.
