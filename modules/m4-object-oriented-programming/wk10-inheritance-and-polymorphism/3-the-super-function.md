# The super() Function

When a child class overrides `__init__`, the parent's `__init__` no longer runs, and the attributes it creates are missing. The child needs a way to run the parent's version as well as its own.

## The super() Function

> **`super()` gives a child class access to the methods of its parent class.**

`super().method_name()` calls the parent's version of a method, even when the child has overridden it.

## Calling the Parent's __init__ Method

**Syntax:**

```python
class ChildClass(ParentClass):
    def __init__(self, parent_parameters, child_parameters):
        super().__init__(parent_parameters)
        self.attribute = value
```

**Example:**

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, course):
        super().__init__(name)
        self.course = course

student1 = Student("Ravi", "Python")
print(student1.name)
print(student1.course)
student1.display_name()
```

**Output:**

```
Ravi
Python
Name: Ravi
```

`super().__init__(name)` runs `Person`'s `__init__`, which sets `self.name`. `Student`'s `__init__` then sets `self.course`. The object has both attributes, and `self.name = name` is still written only once, in `Person`.

Three points about this call:

- **`self` is not passed.** `super()` supplies the current object automatically.
- **Only the parent's parameters are passed.** `Person`'s `__init__` takes `name`, so only `name` is passed. `course` belongs to `Student`.
- **It is usually the first line** of the child's `__init__`, so the parent's attributes exist before the child adds its own.

## Calling the Parent's Other Methods

`super()` works with any method, not only `__init__`. This lets a child **extend** a parent's method: run the parent's version, then add to it.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, course):
        super().__init__(name)
        self.course = course

    def display_name(self):
        super().display_name()
        print("I study", self.course)

student1 = Student("Ravi", "Python")
student1.display_name()
```

**Output:**

```
Name: Ravi
I study Python
```

`Student`'s `display_name()` first calls `Person`'s `display_name()`, which prints the name. It then prints the course. Any change to `Person`'s `display_name()` automatically applies to students too.

| | Overriding Without `super()` | Overriding With `super()` |
| --- | --- | --- |
| Parent's method | Does not run | Runs |
| Result | The child **replaces** the parent's behaviour | The child **extends** the parent's behaviour |

## A Child Class Without __init__

A child class that has no extra attributes does not need its own `__init__`. It inherits the parent's `__init__` and works without `super()`.

```python
class Person:
    def __init__(self, name):
        self.name = name

class Teacher(Person):
    def teach(self):
        print(self.name, "is teaching")

teacher1 = Teacher("Meena")
teacher1.teach()
```

**Output:**

```
Meena is teaching
```

`super().__init__()` is needed only when the child **defines** its own `__init__`.

## Further Reading

- 📎 **Official Python reference for `super()`** — https://docs.python.org/3/library/functions.html#super

Each example so far has one parent and its children. Classes can also be connected in other arrangements. In the next topic, **Types of Inheritance**, you'll learn the different ways classes can inherit from one another.
