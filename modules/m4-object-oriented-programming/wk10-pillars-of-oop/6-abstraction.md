# Abstraction

A parent class often expects every child class to provide a certain method. If a new child class forgets to write that method, Python does not complain, and the program can quietly give wrong results.

Object-oriented programming solves this through **abstraction**. The parent class declares which methods are required, and each child class provides its own version. The code using these classes then needs to know only **what** each method does, not **how** it works.

> **Abstraction means showing only the essential features of an object and hiding the details of how they work.**

Think of a mobile phone. You press the call button to make a call. You do not need to know how the phone sends the signal. The call button is the essential feature, and the signal process is the hidden detail.

In Python, abstraction can also be achieved through ordinary functions, classes and public methods that hide internal details. **Abstract classes** are especially useful when related classes must follow a common design.

## Why Abstract Classes Are Useful

Imagine building a program that calculates the area of different shapes. Every shape should have an `area()` method, but each shape calculates its area differently.

Without an abstract class, a child class might forget to implement `area()`:

```python
class Shape:
    def area(self):
        pass

class Rectangle(Shape):
    pass

rectangle = Rectangle()
print(rectangle.area())
```

**Output:**

```
None
```

The program runs, but it does not calculate an area. `Rectangle` uses the inherited `area()`, which only contains `pass`, so it returns `None`. **Python does not warn us that `Rectangle` has not implemented the required method.**

An abstract class helps prevent this problem. Python raises a **`TypeError`** if we try to create an object from a class that still has unimplemented abstract methods.

## Abstract Classes and Abstract Methods

> **An abstract method is a method declared in an abstract class that a concrete class must implement if the method has not already been implemented by an inherited class.**

A **concrete class** is a class that objects can be created from.

> **An abstract class is a class that contains one or more abstract methods. Objects cannot be created from it while those abstract methods remain unimplemented.**

An abstract class acts as a **plan** for its child classes. It declares methods that concrete child classes must implement, while allowing each class to decide how those methods work.

Python provides abstract classes through its built-in **`abc` module**. The name `abc` stands for **Abstract Base Classes**. This module provides two things:

| Name | Purpose |
| --- | --- |
| `ABC` | The base class used to create an abstract class in this pattern. |
| `@abstractmethod` | A line written above a method to mark it as an abstract method. |

The line `@abstractmethod` is a **decorator**. Decorators are covered in depth in **Decorators**. For now, use `@abstractmethod` as fixed syntax.

## Creating an Abstract Class

**Syntax:**

```python
from abc import ABC, abstractmethod

class ClassName(ABC):
    @abstractmethod
    def method_name(self):
        pass
```

- `from abc import ABC, abstractmethod` imports both names from the `abc` module.
- `class ClassName(ABC):` makes the class an abstract class by inheriting from `ABC`.
- `@abstractmethod` marks `method_name()` as an abstract method.
- `pass` is a placeholder, because this example leaves the method's code to the child classes. Python also allows an abstract method to contain code, but a concrete child class must still provide its own version of it.

## Implementing Abstract Methods

A child class must implement the abstract methods declared by its parent class before its objects can be created. An abstract class can also contain **concrete methods** that child classes can use directly.

> **A concrete method is a normal method with complete code. Child classes inherit it and can use it directly.**

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    def describe(self):
        print("This shape has an area of", self.area())

class Rectangle(Shape):
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side

shapes = [Rectangle(4, 5), Square(3)]

for shape in shapes:
    shape.describe()
```

**Output:**

```
This shape has an area of 20
This shape has an area of 9
```

- `Shape` is the abstract class. It says that every shape **must** have an `area()` method, but it does not say how to calculate it.
- `Rectangle` and `Square` each provide their own `area()` method.
- `describe()` is a concrete method. It is written once in `Shape`, and both child classes inherit it.
- When `describe()` calls `self.area()`, Python runs the `area()` method of the object's own class.

The code using these objects only calls `describe()`. It does not need to know how each shape calculates its area. This is abstraction.

> **Note:** If a child class does not implement an inherited abstract method, Python raises a **`TypeError`** when you try to create an object from that class. For example, if `Rectangle` did not implement `area()`, `Rectangle()` would raise an error similar to this:
>
> ```
> TypeError: Can't instantiate abstract class Rectangle without an implementation for abstract method 'area'
> ```
>
> "Instantiate" means "create an object from". The exact wording of the message can vary between Python versions.

## Abstraction and Encapsulation

Abstraction and encapsulation are often confused. They work together, but they have different goals.

| | Abstraction | Encapsulation |
| --- | --- | --- |
| Main goal | Hides the details of how something works and shows only what it does | Keeps data and methods together and helps control access to an object's data |
| Focus | **What** an object can do, without the user needing to know how it works | **How** an object's data is managed |
| Common Python techniques | Abstract classes and abstract methods | Naming conventions such as `_` and `__`, and methods that manage access to attributes, such as getters and setters |

## Rules to Remember When Implementing Abstraction

1. **Import the required names:** Use `from abc import ABC, abstractmethod`.
2. **Create an abstract class:** Inherit from `ABC`.
3. **Declare abstract methods:** Use `@abstractmethod` above each method that child classes must implement.
4. **Implement the required methods:** A class must implement all inherited abstract methods that remain unimplemented before you can create objects from it.
5. **Use concrete methods when needed:** An abstract class can also contain normal methods that child classes inherit and reuse.
6. **Do not create objects from an incomplete abstract class:** Python raises a **`TypeError`** if abstract methods remain unimplemented.

## Further Reading

- 📎 **Abstraction in Python with examples** — https://www.tutorialspoint.com/python/python_abstraction.htm
- 📎 **Official Python reference for the `abc` module** — https://docs.python.org/3/library/abc.html

The child classes in this topic use `__init__()`, a method whose name starts and ends with two underscores. Python has many more of these special methods. In the next topic, **Dunder Methods**, you'll learn how they let your objects work with built-in functions and operators such as `print()` and `+`.
