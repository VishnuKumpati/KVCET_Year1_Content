# Abstraction

A list is sorted by calling `marks.sort()`. The method puts the elements in order, and using it requires no knowledge of how the sorting is done inside. Only **what** the method does matters, not **how** it does it.

## Abstraction

> **Abstraction means showing only what an object does and hiding how it does it.**

In a class hierarchy, abstraction is achieved by a parent class that lists the methods every child must have, without writing how they work. Each child class then provides the details.

## The Problem

A `Shape` parent class has an `area` method. Every shape should define its own version of `area`.

```python
class Shape:
    def area(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side

class Rectangle(Shape):
    def __init__(self, length, width):
        self.length = length
        self.width = width

shapes = [Square(4), Rectangle(5, 3)]

for shape in shapes:
    print(shape.area())
```

**Output:**

```
16
None
```

`Rectangle` does not define `area`, so it inherits the empty method from `Shape`, which returns `None`. Python raises no error, so the mistake goes unnoticed. A plain `Shape()` object can also be created, although a shape without any measurements has no area.

## Abstract Classes

> **An abstract class is a class that cannot be used to create objects. It exists only to be inherited from.**

> **An abstract method is a method declared in an abstract class without a working body. Every child class must define it.**

Python provides abstract classes through the built-in **`abc`** module, short for Abstract Base Classes.

**Syntax:**

```python
from abc import ABC, abstractmethod

class ClassName(ABC):
    @abstractmethod
    def method_name(self):
        pass
```

- `from abc import ABC, abstractmethod` makes `ABC` and `abstractmethod` available in the program.
- Inheriting from **`ABC`** makes the class abstract.
- The line **`@abstractmethod`**, written directly above a method, marks it as an abstract method. Use it as fixed syntax.

## Creating an Abstract Class

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

shape1 = Shape()
```

**Output:**

```
TypeError: Can't instantiate abstract class Shape without an implementation for abstract method 'area'
```

**To instantiate** a class means to create an object from it. Python refuses to create a `Shape` object, because `Shape` is abstract.

## A Child Class Must Define Every Abstract Method

A child class that does not define the abstract method also cannot be used to create objects.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, length, width):
        self.length = length
        self.width = width

rectangle1 = Rectangle(5, 3)
```

**Output:**

```
TypeError: Can't instantiate abstract class Rectangle without an implementation for abstract method 'area'
```

The error appears as soon as the object is created, and it names both the class and the missing method. Once `Rectangle` defines `area`, it works normally.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side

class Rectangle(Shape):
    def __init__(self, length, width):
        self.length = length
        self.width = width

    def area(self):
        return self.length * self.width

shapes = [Square(4), Rectangle(5, 3)]

for shape in shapes:
    print(shape.area())
```

**Output:**

```
16
15
```

`Shape` states **what** every shape must do: calculate its area. `Square` and `Rectangle` define **how**.

## Normal Methods in an Abstract Class

An abstract class can also contain normal methods. Child classes inherit them as usual, and these methods can call the abstract methods.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    def describe(self):
        print("This shape has an area of", self.area())

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side

square1 = Square(4)
square1.describe()
```

**Output:**

```
This shape has an area of 16
```

`describe` is written once in `Shape`. It calls `self.area()`, which runs the `Square` version. Because `area` is abstract, every shape object is guaranteed to have it.

| | Encapsulation | Abstraction |
| --- | --- | --- |
| Hides | An object's **data** | **How** a method works |
| Achieved with | Private attributes and getter and setter methods | Abstract classes and abstract methods |

## Further Reading

- 📎 **Official Python reference for the `abc` module** — https://docs.python.org/3/library/abc.html

`__init__` is one method Python calls automatically, without it being called by name. Python has many more such methods. In the next topic, **Dunder Methods**, you'll learn how they let objects work with built-in functions and operators.
