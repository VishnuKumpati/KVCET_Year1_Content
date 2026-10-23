# Decorators

Suppose you want to print a message before and after a function runs. You could add those lines to every function, but that means repeating the same code. **Decorators** solve this.

> **A decorator is a function that adds or changes a function's behaviour without requiring changes to the original function's code.**

In this topic, you'll build a decorator step by step. You've already seen decorators such as `@classmethod` in **Class Attributes and Class Methods** and `@abstractmethod` in **Abstraction**. Now, let's understand how they work.

## Passing a Function to Another Function

Decorators depend on one simple fact: in Python, **a function can be passed to another function**, just like a value.

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

`greet` is passed to `run_function()` **without parentheses**:

- `greet` refers to the function.
- `greet()` calls the function.

Inside `run_function()`, `func()` calls the function that was passed in. You'll use this idea to build a decorator that adds behaviour around another function.

## Creating a Decorator

A decorator takes the original function, creates a **wrapper** to run extra code around it, and returns the wrapper.

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

1. `func` refers to the original function, `say_hello`.
2. `wrapper()` runs the extra code and calls `func()` in between.
3. `return wrapper` gives the wrapper back. The line `say_hello = my_decorator(say_hello)` stores it in `say_hello`, so calling `say_hello()` now runs the wrapper.

Notice that `return wrapper` returns the function itself **without calling it**. The wrapper runs later, when you call `say_hello()`.

## The `@` Syntax

Python provides a shorter way to apply a decorator. Write `@` followed by the decorator's name directly above the function.

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

**Writing `@my_decorator` above a function is a shorter way of writing `say_hello = my_decorator(say_hello)`.** Python applies the decorator when the function is defined.

## Decorating Functions with Arguments and Return Values

Our first wrapper works only with functions that take no arguments. To support functions with different inputs, we use `*args` and `**kwargs` to accept arguments and pass them to the original function. The wrapper should also return the original function's result when the caller needs it.

```python
def show_message(func):
    def wrapper(*args, **kwargs):
        print("Starting the calculation")
        result = func(*args, **kwargs)
        return result
    return wrapper

@show_message
def add(a, b):
    return a + b

print(add(10, 20))
```

**Output:**

```
Starting the calculation
30
```

- `*args` accepts positional arguments, such as `10` and `20`. `**kwargs` accepts keyword arguments, such as `a=10` and `b=20`. Together, they let the wrapper handle different function arguments.
- `return result` sends the answer back. Without it, `add(10, 20)` would give `None`.

## Decorators Used with Classes

Python uses decorators to give methods special behaviour. You have already used two of them:

- **`@classmethod`**, in **Class Attributes and Class Methods**, defines a method that receives the class as `cls`.
- **`@abstractmethod`**, in **Abstraction**, marks a method that a concrete class must implement before its objects can be created.

Two more are commonly used with classes.

### `@staticmethod`

`@staticmethod` defines a method that behaves like a regular function defined inside a class. It does not automatically receive the object (`self`) or the class (`cls`) as its first argument.

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
| **Instance method** | None | `self`, the object |
| **Class method** | `@classmethod` | `cls`, the class |
| **Static method** | `@staticmethod` | None |

### `@property`

`@property` lets you access a method **like an attribute**, without parentheses. A matching setter lets you control how the value is changed.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks

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

- `@property` makes `marks` a **getter**. Writing `student1.marks` runs the method automatically.
- `@marks.setter` defines the **setter**. Writing `student1.marks = -10` runs the check, so the invalid value is rejected.

All these decorators use the **same `@` syntax** you have just learned.

## Points to Remember

- **A decorator is applied when the function is defined.** Its wrapper runs each time the decorated function is called.
- **Use `*args` and `**kwargs`** when the wrapper needs to accept different arguments.
- **Return the original function's result** from the wrapper when the caller needs that result.

## Further Reading

- 📎 **Official Python glossary entry for decorators** — https://docs.python.org/3/glossary.html#term-decorator

Everything a program has stored so far exists only while it runs. Next week, you'll learn how to save data to files and read it back.
