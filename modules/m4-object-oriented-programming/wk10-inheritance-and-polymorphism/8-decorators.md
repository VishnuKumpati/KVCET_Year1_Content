# Decorators

`@classmethod` and `@abstractmethod` are written above a method to change how it behaves. Each of these lines is a **decorator**. Decorators work because of one fact about Python functions: **a function is an object**, just like a number or a list.

## Functions Are Objects

A function can be stored in a variable and passed to another function as an argument.

```python
def greet():
    print("Welcome to City College")

say_hello = greet
say_hello()

def run_twice(func):
    func()
    func()

run_twice(greet)
```

**Output:**

```
Welcome to City College
Welcome to City College
Welcome to City College
```

`say_hello = greet` has no brackets after `greet`, so the function is **not called**. The variable `say_hello` now refers to the same function, and `say_hello()` calls it. `run_twice(greet)` passes the function itself to `run_twice`, which calls it twice.

A function can also be defined inside another function and returned from it.

## Decorators

> **A decorator is a function that takes another function, adds some behaviour to it, and returns the new version.** The original function's code does not change.

**Syntax:**

```python
def decorator_name(func):
    def wrapper():
        # code that runs before the function
        func()
        # code that runs after the function
    return wrapper
```

- `func` is the function being decorated.
- `wrapper` is a new function, defined inside the decorator. It runs extra code and calls `func`.
- The decorator returns `wrapper` without calling it.

## Writing a Decorator

```python
def show_border(func):
    def wrapper():
        print("----------")
        func()
        print("----------")
    return wrapper

def greet():
    print("Welcome to City College")

greet = show_border(greet)
greet()
```

**Output:**

```
----------
Welcome to City College
----------
```

`show_border(greet)` returns `wrapper`, and `greet = show_border(greet)` stores it back in the variable `greet`. Calling `greet()` now runs `wrapper`, which prints a line, calls the original `greet`, and prints another line.

## The @ Syntax

Writing `greet = show_border(greet)` after every function is repetitive. Python provides a shorter form: the decorator's name with `@`, written directly above the function.

```python
def show_border(func):
    def wrapper():
        print("----------")
        func()
        print("----------")
    return wrapper

@show_border
def greet():
    print("Welcome to City College")

greet()
```

**Output:**

```
----------
Welcome to City College
----------
```

`@show_border` above `def greet()` means exactly the same as `greet = show_border(greet)` after it. The decorator can be applied to any number of functions by adding the same line above each one.

## Decorating a Function with Parameters

When the decorated function takes arguments, `wrapper` must accept the same arguments and pass them on to `func`.

```python
def show_border(func):
    def wrapper(name):
        print("----------")
        func(name)
        print("----------")
    return wrapper

@show_border
def greet(name):
    print("Welcome,", name)

greet("Ravi")
```

**Output:**

```
----------
Welcome, Ravi
----------
```

`greet("Ravi")` calls `wrapper("Ravi")`, which passes `"Ravi"` on to the original `greet`.

## Built-in Decorators for Methods

Python provides built-in decorators that change how a method inside a class works. They are used as they are, without writing the decorator function.

### @staticmethod

> **A static method is a method that receives neither the object (`self`) nor the class (`cls`). It is a normal function placed inside a class because it is related to that class.**

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    @staticmethod
    def is_valid_marks(marks):
        return 0 <= marks <= 100

print(Student.is_valid_marks(85))
print(Student.is_valid_marks(120))
```

**Output:**

```
True
False
```

`is_valid_marks` only checks a number. It does not use any object's data or class attribute, so it needs neither `self` nor `cls`. It is called through the class name.

### @property

Getter and setter methods control access to a private attribute, but they are called like methods: `student1.get_marks()` and `student1.set_marks(90)`. The **`@property`** decorator gives the same control while letting the attribute be read and changed with normal attribute syntax.

> **A property is a method that is accessed like an attribute.**

**Syntax:**

```python
class ClassName:
    @property
    def attribute_name(self):
        return self.__attribute_name

    @attribute_name.setter
    def attribute_name(self, value):
        self.__attribute_name = value
```

- The method decorated with **`@property`** is the getter. It runs when the attribute is read.
- The method decorated with **`@attribute_name.setter`** is the setter. It runs when a value is assigned to the attribute.
- Both methods have the **same name**.

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

student1.marks = 90
print(student1.marks)

student1.marks = -10
print(student1.marks)
```

**Output:**

```
90
Invalid marks
90
```

`student1.marks = 90` looks like a normal assignment, but it runs the setter, which checks the value. `print(student1.marks)` runs the getter. The value `-10` fails the check, so the marks stay at 90.

### @classmethod

**`@classmethod`** is also a built-in decorator. It changes a method so that its first argument is the **class** (`cls`) instead of the object (`self`).

```python
class Student:
    total_students = 0

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
        Student.total_students = Student.total_students + 1

    @classmethod
    def display_count(cls):
        print("Total students:", cls.total_students)

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

Student.display_count()
```

**Output:**

```
Total students: 2
```

`display_count` works with the class attribute `total_students`, so it receives the class as `cls`.

| Method Type | Decorator | First Parameter | Used For |
| --- | --- | --- | --- |
| Instance method | None | `self` (the object) | Working with one object's data |
| Class method | `@classmethod` | `cls` (the class) | Working with class attributes |
| Static method | `@staticmethod` | None | A related task that needs neither the object nor the class |

## Further Reading

- 📎 **Official Python glossary entry for decorators** — https://docs.python.org/3/glossary.html#term-decorator
- 📎 **Official Python reference for `property`** — https://docs.python.org/3/library/functions.html#property
- 📎 **Official Python reference for `staticmethod`** — https://docs.python.org/3/library/functions.html#staticmethod

A decorator wraps extra code around a function. Some tasks need extra code to run before and after a block of statements, such as opening and closing a resource. In the next topic, **Context Managers**, you'll learn how the `with` statement handles this.
