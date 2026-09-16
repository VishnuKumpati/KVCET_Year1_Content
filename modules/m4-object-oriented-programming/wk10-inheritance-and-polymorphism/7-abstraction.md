# Abstraction

**Abstraction** is exposing what something does while hiding how it does it.

It is already familiar. `marks.sort()` puts a list in order, and using it requires no knowledge of the algorithm, its complexity, or the C code underneath. The method name is the whole interface. If the implementation were replaced tomorrow, nothing that calls `sort()` would change.

Every class you write offers the same bargain. `account.withdraw(200)` says what happens; whether the balance is an integer, a decimal or a database row is the account's business.

This chapter is about the stricter form: defining an interface that subclasses are **required** to implement.

## The Problem

Polymorphism depends on a method existing on every type involved. Nothing so far guarantees it:

```python
class Shape:
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

class Triangle(Shape):
    def __init__(self, base, height):
        self.base = base
        self.height = height

shapes = [Circle(2), Triangle(3, 4)]
for shape in shapes:
    print(shape.area())
```

**Output:**

```
12.56636
None
```

`Triangle` forgot `area`, inherited the empty one from `Shape`, and returned `None`. No error was raised. The `None` travels onwards and fails somewhere else — in a `sum()`, a comparison, or a format specification — a long way from the class that caused it.

Worse, `Shape()` can be created directly:

```python
print(Shape().area())
```

**Output:**

```
None
```

A bare `Shape` is not a shape. It has no dimensions and no area, and it exists only to be inherited from.

## Abstract Base Classes

The `abc` module fixes both problems. `ABC` is the class to inherit from, and `@abstractmethod` marks a method that subclasses must provide:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

Two things are now enforced.

**The abstract class cannot be instantiated:**

```python
shape = Shape()
```

**Output:**

```
TypeError: Can't instantiate abstract class Shape without an implementation for abstract method 'area'
```

**A subclass that does not implement every abstract method cannot be instantiated either:**

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Triangle(Shape):
    def __init__(self, base, height):
        self.base = base
        self.height = height

triangle = Triangle(3, 4)
```

**Output:**

```
TypeError: Can't instantiate abstract class Triangle without an implementation for abstract method 'area'
```

The error names the class and the missing method, and it arrives the first time a Triangle is created rather than wherever the `None` eventually caused trouble.

A subclass that implements the method works normally:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

print(Circle(2).area())
```

**Output:**

```
12.56636
```

## Abstract and Concrete Together

An abstract class is not limited to abstract methods. It can provide ordinary ones, and those can call the abstract ones:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    def describe(self):
        return f"{type(self).__name__} with area {self.area():.2f}"

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side ** 2

for shape in [Circle(2), Square(3)]:
    print(shape.describe())
```

**Output:**

```
Circle with area 12.57
Square with area 9.00
```

`describe()` is written once and works for every shape. It calls `self.area()` without knowing how any shape computes it — and the abstract method guarantees that every shape can.

This is the useful shape of an abstract base class: the parent supplies the logic that is the same everywhere, and demands the pieces that differ.

`__init__` can be abstract too, and an abstract class may have a perfectly ordinary `__init__` for subclasses to call through `super()`.

## Abstraction Against Encapsulation

The two are often confused because both involve hiding.

**Encapsulation** hides *data*. It keeps attributes behind methods and properties so the object's state cannot be corrupted from outside.

**Abstraction** hides *implementation*. It presents an interface — these are the operations — without committing to how they are carried out.

An account's `_balance` being private is encapsulation. `withdraw()` being the only meaningful way to take money out, regardless of what happens inside, is abstraction. They work together and answer different questions.

## When to Use an Abstract Base Class

Use one when several classes must share an interface and it would be a bug for one to omit part of it — a set of shapes that must all compute area, a set of payment methods that must all process a charge, a set of exporters that must all write a file.

Do not use one for a two-class hierarchy where the relationship is obvious, and do not create an abstract class with a single implementation. Duck typing covers most polymorphism in Python perfectly well, and an ABC adds a real constraint that should be there for a reason.

The signal to reach for one is that a missing method would fail *silently* rather than loudly. Where the failure would be a clear `AttributeError` at the point of the mistake, duck typing is already sufficient.

## Further Reading

- **Official reference for the `abc` module** — https://docs.python.org/3/library/abc.html
- **Abstract base classes in practice** — https://realpython.com/python-interface/

An abstract base class defines an interface, cannot be instantiated, and refuses to build any subclass that has not implemented every abstract method. Next, the methods Python itself calls on your objects.
