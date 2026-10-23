# Encapsulation

A medicine capsule keeps the medicine enclosed inside its shell, so you don't handle the medicine directly. In a similar way, **encapsulation** keeps an object's data together with the methods that work on it and provides a controlled way to access or change that data. Encapsulation is the first pillar of OOP.

> **Encapsulation means keeping an object's data and the methods that work on it together in a class, while controlling how the data can be accessed or modified.**

Consider a `Student` object that stores marks. Marks should stay between 0 and 100, but any part of the program can change them directly, even to `-10`. Encapsulation allows the class to control such changes and prevent invalid values.

## The Problem Without Encapsulation

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student1.marks = -10

print(student1.marks)
```

**Output:**

```
-10
```

Python accepts `-10` without any check, although marks cannot be negative. Nothing in the class stops the wrong value. The rest of this topic shows how to prevent this.

## Access Levels in Python

Python does not have keywords that block access to data. Instead, the way an attribute's **name begins** shows how it is meant to be used. In the table, `attribute` stands for any attribute name.

| Access Level | How the Name Begins | Meaning |
| --- | --- | --- |
| **Public** | `attribute` | No underscore. Can be used from anywhere in the program. |
| **Protected** | `_attribute` | One underscore. Meant for use inside the class, but Python does **not** block outside access. |
| **Private** | `__attribute` | Two underscores. Cannot normally be accessed by that name from outside the class. |

The same rules apply to **methods**. The sections below show the protected and private levels using the `Student` class.

## Protected Attributes

One underscore is only a **signal** to other programmers. It means "please use this inside the class only", but Python does not stop anyone.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self._marks = marks

student1 = Student("Ravi", 85)
print(student1._marks)
```

**Output:**

```
85
```

The value is printed, because Python does not block `_marks`.

## Private Attributes

Two leading underscores make an attribute **private** by convention and name mangling. It can be accessed normally **inside** the class, but accessing it using the same name from **outside** the class raises an `AttributeError`.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks

    def show_marks(self):
        print("Marks:", self.__marks)

student1 = Student("Ravi", 85)
student1.show_marks()

try:
    print(student1.__marks)
except AttributeError:
    print("Cannot access __marks from outside the class")
```

**Output:**

```
Marks: 85
Cannot access __marks from outside the class
```

- `show_marks()` is **inside** the class, so it can use `self.__marks`.
- `student1.__marks` is **outside** the class, so Python raises an `AttributeError`. The `except` block catches it and prints a message.

## Name Mangling

Python makes a private attribute hard to reach by **renaming** it. This is called name mangling.

> **Name mangling is a process in which Python changes the name of an attribute that starts with two underscores by adding an underscore and the class name before it.**

Name mangling does not apply to names that also **end** with two underscores, such as `__init__`.

In the `Student` class:

| Name Written in the Class | Name Python Actually Uses |
| --- | --- |
| `__marks` | `_Student__marks` |

Because the name was changed, `student1.__marks` does not work from outside the class. The renamed version does:

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks

student1 = Student("Ravi", 85)
print(student1._Student__marks)
```

**Output:**

```
85
```

This shows that a private attribute is **not completely hidden**. Name mangling only prevents **accidental** access. **Do not use the renamed version in real programs**, because it breaks the protection the class is trying to give.

## Getter and Setter Methods

When a class needs to control how an attribute is read or changed, it can provide methods for those operations. A getter returns the value, while a setter can validate a new value before updating it:

| Method | Purpose | Example in This Topic |
| --- | --- | --- |
| **Getter** | **Returns** the value of an attribute | `get_marks()` |
| **Setter** | **Changes** the value of an attribute, usually after checking that the new value is valid | `set_marks()` |

In this example, the getter is named `get_marks()` and the setter is named `set_marks()`. These are common naming conventions, not rules enforced by Python.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        if 0 <= marks <= 100:
            self.__marks = marks
        else:
            raise ValueError("Marks must be between 0 and 100")

    def get_marks(self):
        return self.__marks

    def set_marks(self, marks):
        if 0 <= marks <= 100:
            self.__marks = marks
        else:
            print("Invalid marks")

student1 = Student("Ravi", 85)

student1.set_marks(90)
print(student1.get_marks())

student1.set_marks(-10)
print(student1.get_marks())
```

**Output:**

```
90
Invalid marks
90
```

### How Each Method Works

- **`__init__()`** checks the marks when the object is created. The condition `0 <= marks <= 100` is true only when the marks are between 0 and 100, including both values. If the marks are invalid, `raise ValueError(...)` stops the object from being created and reports the error.
- **`get_marks()`** returns `self.__marks`. Code outside the class can **read** the marks without touching the private attribute.
- **`set_marks()`** receives a new value and checks it with the same condition. If the value is valid, it replaces the old marks. If not, it prints `Invalid marks` and **leaves the old marks unchanged**.

`set_marks(90)` is accepted, so the marks become `90`. `set_marks(-10)` is rejected, so the marks stay at `90`. This is encapsulation in practice: **the class decides how its data can be changed**.

Not every attribute needs a getter or setter. In Python, public attributes can be used directly when no special control is needed. Getter and setter methods are useful when the class needs to **check or control** how data is changed.

## Points to Remember

- **Encapsulation keeps data and methods together** and controls how the data is accessed and changed.
- **One underscore (`_marks`)** is a signal for internal use. Python does not block access.
- **Two underscores (`__marks`)** make an attribute private. Python renames it to `_Student__marks`, which is called name mangling.
- **Getters read data and setters change it**, often after checking that the new value is valid.

## Further Reading

- 📎 **Official Python guide to private variables and name mangling** — https://docs.python.org/3/tutorial/classes.html#private-variables

Students and teachers have some details in common, such as a name, but they also have different responsibilities. In the next topic, **Inheritance**, you'll learn how one class can reuse the attributes and methods of another.
