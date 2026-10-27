# Decorators

Functions let us reuse code, but what if we want to add the same behaviour to several functions without changing their original code?

For example, we might want to print a message before a function runs, check whether a user is authorised, or record how long a function takes to execute. Adding this logic separately to every function creates repetition.

Python provides **decorators** to solve this problem. They let us add or modify behaviour by wrapping a function, keeping the additional logic separate from the function's main task.

> **A decorator is a callable that takes another function or object and returns a decorated result. In the function decorators covered here, it returns a function with added or modified behaviour.**

You've already used decorators such as `@classmethod` and `@abstractmethod`. Now, let's understand how they work and how to create your own.

## Passing a Function to Another Function

In Python, a function can be passed to another function, just like a number or a string.

```python
def greet():
    print("Hello!")

def run_function(func):
    func()

run_function(greet)
```

**Output:**

```
Hello!
```

- `greet` is written **without parentheses**, so the function itself is passed, not its result.
- Inside `run_function()`, `func()` calls the function that was passed in.

## Returning a Function from a Function

A function can also be defined inside another function and returned from it.

```python
def outer():
    def inner():
        print("Hello from inner")
    return inner

greeting = outer()
greeting()
```

**Output:**

```
Hello from inner
```

- `inner()` is defined inside `outer()`.
- `return inner` returns the function itself, **without calling it**.
- `greeting` now refers to `inner`, so `greeting()` runs it.

A decorator combines both ideas: it **receives** a function and **returns** a new one.

## Creating a Decorator

```python
def my_decorator(func):
    def wrapper():
        print("Before the function runs")
        func()
        print("After the function runs")
    return wrapper

def say_hello():
    print("Hello!")

say_hello = my_decorator(say_hello)
say_hello()
```

**Output:**

```
Before the function runs
Hello!
After the function runs
```

1. `my_decorator()` receives the original function as `func`.
2. It defines a **wrapper** function that runs extra code before and after calling `func()`.
3. It returns the wrapper.
4. `say_hello = my_decorator(say_hello)` replaces `say_hello` with the wrapper. So calling `say_hello()` now runs the extra code as well.

The original `say_hello()` function was not changed. The decorator only added behaviour around it.

## Applying a Decorator with `@`

Python provides a shorter way to apply a decorator: write `@` and the decorator's name directly above the function.

```python
def my_decorator(func):
    def wrapper():
        print("Before the function runs")
        func()
        print("After the function runs")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

**Output:**

```
Before the function runs
Hello!
After the function runs
```

| With `@` | Without `@` |
| --- | --- |
| `@my_decorator` above `def say_hello():` | `say_hello = my_decorator(say_hello)` after the function |

Both do exactly the same thing. Python applies the decorator **once**, when the function is defined. The wrapper then runs **every time** the function is called.

## Decorating Functions with Arguments

The wrapper above takes no arguments, so it works only with functions that take no arguments. To work with any function, the wrapper uses `*args` and `**kwargs`, covered in **Polymorphism**, and returns the original function's result.

```python
def show_message(func):
    def wrapper(*args, **kwargs):
        print("Calculating...")
        return func(*args, **kwargs)
    return wrapper

@show_message
def add(a, b):
    return a + b

print(add(10, 20))
```

**Output:**

```
Calculating...
30
```

- `*args` and `**kwargs` let the wrapper accept any arguments, such as `10` and `20`.
- `func(*args, **kwargs)` passes those arguments on to the original `add()`.
- `return` sends back the result. Without it, `add(10, 20)` would give `None`.

## Decorators Used in Classes

Python provides several built-in decorators for methods. Two are already familiar:

- **`@classmethod`** creates a method that receives the class as `cls`.
- **`@abstractmethod`** marks a method that concrete child classes must implement.

Two more are commonly used.

### `@staticmethod`

`@staticmethod` creates a method that does not receive the object (`self`) or the class (`cls`). It works like a normal function placed inside a class, because it belongs there logically.

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b

print(Calculator.add(10, 20))
```

**Output:**

```
30
```

| Method Type | Decorator | First Parameter |
| --- | --- | --- |
| Instance method | None | `self`, the object |
| Class method | `@classmethod` | `cls`, the class |
| Static method | `@staticmethod` | None |

### `@property`

In **Encapsulation**, marks were read and changed through `get_marks()` and `set_marks()`. `@property` gives the same control, but lets you use the method **like a normal attribute**.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    @property
    def marks(self):
        return self.__marks

    @marks.setter
    def marks(self, value):
        if 0 <= value <= 100:
            self.__marks = value
        else:
            print("Invalid marks")

student1 = Student("Ravi", 85)
print(student1.marks)

student1.marks = -10
print(student1.marks)
```

**Output:**

```
85
Invalid marks
85
```

- `@property` lets you read the marks using `student1.marks` instead of calling a getter method with parentheses.
- `@marks.setter` lets you control what happens when someone assigns a value to `student1.marks`. Here, the setter rejects values outside the range 0–100.
- In `__init__()`, `self.marks = marks` also uses the setter, so the marks are checked when the object is created.

This example assumes the initial marks are valid. If invalid marks are supplied during object creation, the setter rejects them before `__marks` is set.

## Points to Remember

- **A decorator takes another function or object and returns a decorated result.** In the function decorators covered here, it returns a function with added or modified behaviour.
- **`@decorator` above a function** is a shorter way of writing `function = decorator(function)`.
- **Use `*args` and `**kwargs`** to let a wrapper accept different arguments, and return the original function's result when that result needs to be preserved.
- **`@staticmethod` and `@property`** are built-in decorators commonly used in classes.

## Further Reading

- 📎 **Decorators in Python** — https://www.tutorialspoint.com/python/python_decorators.htm
- 📎 **Official Python glossary entry for decorators** — https://docs.python.org/3/glossary.html#term-decorator

Everything a program has stored so far exists only while it runs. Next week, you'll learn how to save data to files and read it back.
