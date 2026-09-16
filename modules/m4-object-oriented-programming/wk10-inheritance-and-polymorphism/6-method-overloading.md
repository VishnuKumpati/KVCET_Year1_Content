# Method Overloading

**Method overloading** is defining several methods with the same name in one class, distinguished by the number or type of their parameters. The language picks the right one at each call.

In Java or C++ a class might hold three `area` methods — one taking a radius, one taking a side, one taking a width and a height — and the compiler chooses by matching the arguments.

**Python does not have method overloading.** This chapter covers why, and what Python does instead.

## Later Definitions Replace Earlier Ones

Writing two methods with the same name is not an error. The second simply replaces the first:

```python
class Greeter:
    def hello(self):
        print("no arguments")

    def hello(self, name):
        print("one argument:", name)

greeter = Greeter()
greeter.hello("Anita")
```

**Output:**

```
one argument: Anita
```

The two-parameter version won because it was defined last. The other is gone:

```python
greeter.hello()
```

**Output:**

```
TypeError: Greeter.hello() missing 1 required positional argument: 'name'
```

No warning was given at definition. The first `hello` was created and then immediately overwritten.

The reason is that `def` is an assignment. `def hello(self):` binds a function to the name `hello` in the class body, and the second `def` binds a different function to the same name — exactly as `x = 1` followed by `x = 2` leaves `x` as `2`. A name holds one thing.

This also means Python cannot choose by argument types, because it does not look at types when calling. That is the same design decision behind duck typing.

## Default Parameters

The first replacement is defaults, which handle "the same operation with some values optional":

```python
def area(width, height=None):
    if height is None:
        return width * width
    return width * height

print(area(4))
print(area(4, 5))
```

**Output:**

```
16
20
```

One function, two ways of calling it. In an overloading language this would be two methods.

`None` is the right default here rather than `0`, because it means "not supplied" and cannot be confused with a real height.

## Variable Numbers of Arguments

`*args` collects any number of positional arguments into a tuple:

```python
def total(*numbers):
    return sum(numbers)

print(total())
print(total(1, 2))
print(total(1, 2, 3))
```

**Output:**

```
0
3
6
```

Three calls with different argument counts, one definition. An overloading language would need a method per count, or a special case of its own.

`**kwargs` does the same for keyword arguments, collecting them into a dictionary:

```python
class Config:
    def __init__(self, **settings):
        self.settings = settings

    def show(self):
        for key, value in self.settings.items():
            print(f"{key}: {value}")

Config(theme="dark", font="serif").show()
```

**Output:**

```
theme: dark
font: serif
```

The star is what does the collecting. `*` gathers positional arguments, `**` gathers keyword ones, and the names `args` and `kwargs` are convention.

## Branching on Type

When behaviour genuinely differs by type, check the type inside one method:

```python
class Box:
    def __init__(self, contents):
        self.contents = contents

    def add(self, item):
        if isinstance(item, list):
            self.contents.extend(item)
        else:
            self.contents.append(item)

box = Box([1, 2])
box.add(3)
box.add([4, 5])
print(box.contents)
```

**Output:**

```
[1, 2, 3, 4, 5]
```

Use this sparingly. A method that branches three or four ways on type is usually two or three methods with honest names — `add_item` and `add_many` say more than one `add` that guesses.

## Alternative Constructors

The one case where overloading is genuinely missed is `__init__`. A class can have only one, and several ways of building an object is a real need.

Python's answer is a **class method**: a method that receives the class rather than an instance, and returns a new object:

```python
class Date:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, text):
        day, month, year = text.split("-")
        return cls(int(day), int(month), int(year))

    def describe(self):
        return f"{self.day}/{self.month}/{self.year}"

print(Date(16, 9, 2026).describe())
print(Date.from_string("16-9-2026").describe())
```

**Output:**

```
16/9/2026
16/9/2026
```

`@classmethod` changes the first parameter from `self` to `cls`, the class itself. `cls(...)` then calls `__init__` in the ordinary way.

Two things make this better than overloading. The alternative constructor has a **name**, so `Date.from_string("16-9-2026")` says what its argument is, where an overloaded `Date("16-9-2026")` would not. And `cls` rather than `Date` means a subclass inherits the method and gets an object of its own type.

Name these `from_something`. It is the convention throughout the standard library.

## Single Dispatch

For the case that really is one operation over unrelated types, the standard library offers `singledispatch`, which picks an implementation by the type of the first argument:

```python
from functools import singledispatch

@singledispatch
def describe(value):
    return f"Something: {value}"

@describe.register
def _(value: int):
    return f"The number {value}"

@describe.register
def _(value: str):
    return f"The text '{value}'"

print(describe(5))
print(describe("hello"))
print(describe([1, 2]))
```

**Output:**

```
The number 5
The text 'hello'
Something: [1, 2]
```

This is overloading in everything but name, added as a library feature rather than a language one. It is worth knowing about and rarely worth reaching for — defaults and `*args` cover almost everything.

## Operator Overloading Is Different

The term **operator overloading** refers to something Python does support: giving `+`, `==`, `<` and the rest a meaning for your own classes.

It is unrelated to method overloading despite the shared word. Method overloading is several methods with one name; operator overloading is one method that an operator calls. The next chapter covers it.

## Summary

| Instead of overloading by | Python uses |
| --- | --- |
| optional parameters | default parameter values |
| varying argument counts | `*args` and `**kwargs` |
| differing argument types | an `isinstance` check, or `singledispatch` |
| several constructors | `@classmethod` alternative constructors |

The common thread is that Python prefers one function with a flexible signature, or several functions with distinct names, over several functions sharing a name.

## Further Reading

- **Official reference for `functools.singledispatch`** — https://docs.python.org/3/library/functools.html#functools.singledispatch
- **Class methods and alternative constructors** — https://realpython.com/instance-class-and-static-methods-demystified/

Python has no method overloading, because a name holds one function; defaults, `*args` and class methods cover what it would be used for. Next, requiring a subclass to provide a method at all.
