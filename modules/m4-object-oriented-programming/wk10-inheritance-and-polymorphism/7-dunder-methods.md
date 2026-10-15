# Dunder Methods

Printing a list shows its elements. Printing a `Student` object shows something much less useful.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
print(student1)
```

**Output:**

```
<__main__.Student object at 0x000001F3D7D61E80>
```

The output shows the class name and a memory address, which changes every time the program runs. A class can control what `print()` shows by defining a **dunder method**.

## Dunder Methods

> **A dunder method is a special method whose name starts and ends with two underscores, such as `__init__`. Python calls it automatically when a matching operation is used.** "Dunder" is short for **d**ouble **under**score.

`__init__` is already a dunder method. It is never called by name; Python runs it when an object is created. Other dunder methods work the same way: each one connects a class to a built-in function or operator.

| Operation | Dunder Method Python Calls |
| --- | --- |
| `Student("Ravi", 85)` | `__init__` |
| `print(student1)` | `__str__` |
| `len(course1)` | `__len__` |
| `student1 == student2` | `__eq__` |
| `point1 + point2` | `__add__` |

## The __str__ Method

`__str__` defines the text shown when an object is printed. It must **return** a string.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def __str__(self):
        return self.name + " scored " + str(self.marks)

student1 = Student("Ravi", 85)
print(student1)
```

**Output:**

```
Ravi scored 85
```

`print(student1)` calls `student1.__str__()` and prints the string it returns.

## The __len__ Method

`__len__` defines what `len()` returns for an object. It must return a whole number.

```python
class Course:
    def __init__(self, title):
        self.title = title
        self.students = []

    def add_student(self, name):
        self.students.append(name)

    def __len__(self):
        return len(self.students)

course1 = Course("Python")
course1.add_student("Ravi")
course1.add_student("Priya")
course1.add_student("Meena")

print(len(course1))
```

**Output:**

```
3
```

`len(course1)` calls `course1.__len__()`, which returns the number of students in the course.

## The __eq__ Method

By default, `==` on two objects is `True` only if they are the **same object**. Two separate objects with the same data are not equal.

`__eq__` defines what `==` compares. Its second parameter, usually named `other`, is the object on the right side of `==`.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def __eq__(self, other):
        return self.name == other.name and self.marks == other.marks

student1 = Student("Ravi", 85)
student2 = Student("Ravi", 85)
student3 = Student("Priya", 92)

print(student1 == student2)
print(student1 == student3)
```

**Output:**

```
True
False
```

`student1 == student2` calls `student1.__eq__(student2)`. Both objects have the same name and marks, so the result is `True`.

## Operator Overloading

> **Operator overloading means giving an operator, such as `+` or `<`, a meaning for objects of a class.** It is done by defining the matching dunder method.

The `+` operator already behaves differently for numbers, strings and lists. Defining `__add__` makes it work for a new class too.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)

    def __str__(self):
        return "(" + str(self.x) + ", " + str(self.y) + ")"

point1 = Point(2, 3)
point2 = Point(4, 1)
point3 = point1 + point2

print(point3)
```

**Output:**

```
(6, 4)
```

`point1 + point2` calls `point1.__add__(point2)`. The method returns a **new** `Point` object, and `point1` and `point2` stay unchanged, just as `2 + 3` does not change `2` or `3`.

Every arithmetic and comparison operator has its own dunder method.

| Operator | Dunder Method |
| --- | --- |
| `+` | `__add__` |
| `-` | `__sub__` |
| `*` | `__mul__` |
| `/` | `__truediv__` |
| `==` | `__eq__` |
| `!=` | `__ne__` |
| `<` | `__lt__` |
| `>` | `__gt__` |
| `<=` | `__le__` |
| `>=` | `__ge__` |

Define an operator only when its meaning is obvious. Adding two points is clear. Adding two students is not.

## Further Reading

- 📎 **Official Python reference for special method names** — https://docs.python.org/3/reference/datamodel.html#special-method-names

The `@abstractmethod` and `@classmethod` lines placed above methods are called decorators. In the next topic, **Decorators**, you'll learn what a decorator is and how to write one.
