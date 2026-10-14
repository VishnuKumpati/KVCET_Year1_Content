# Encapsulation

A `Student` object stores its marks as an attribute. Any part of the program can change that attribute directly, even to a value that makes no sense.

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

Python accepts `-10` without any check, although marks cannot be negative. **Encapsulation** solves this problem.

## Encapsulation

> **Encapsulation means keeping an object's data and the methods that work on it together in a class, and controlling how that data is accessed and changed from outside the class.**

In Python, encapsulation is usually applied in two steps:

1. The data is marked as internal, so that it is not changed directly from outside the class.
2. The class provides methods that read or change the data in a controlled way.

## Access Levels in Python

Python does not have keywords to restrict access to attributes. Instead, the **name** of an attribute shows how it is meant to be used.

| Access Level | Example | Meaning |
| --- | --- | --- |
| Public | `marks` | Can be used from anywhere in the program. |
| Protected | `_marks` | One underscore. According to Python standards, it is meant for internal use only, but Python does not block access from outside the class. |
| Private | `__marks` | Two underscores. Python changes the name internally, so the attribute cannot be reached by this name from outside the class. |

These naming rules apply to **methods** as well as attributes.

## Private Attributes

A private attribute is created by starting its name with **two underscores**.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks
```

This code only defines the class, so running it produces no output.

Inside the class, `self.__marks` works normally. Outside the class, writing `student1.__marks` raises an **`AttributeError`**, because the attribute cannot be found by that name.

Python achieves this by renaming the attribute internally to `_Student__marks`. This renaming is called **name mangling**. The data can still be reached through the changed name, so **private attributes prevent accidental access, not deliberate access**.

## Getter and Setter Methods

Once an attribute is private, the class provides methods to work with it:

- A **getter** is a method that returns the value of a private attribute.
- A **setter** is a method that changes the value of a private attribute, usually after checking that the new value is valid.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks

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

The condition `0 <= marks <= 100` is true only when the marks are between 0 and 100, including both values. `set_marks(90)` passes the check, so the marks change to 90. `set_marks(-10)` fails the check, so the method prints `Invalid marks` and the marks stay at 90.

The intended way to change marks from outside the class is now `set_marks`, which rejects any number outside the range 0 to 100. This is encapsulation in practice: **the class decides how its data can be changed**.

## Further Reading

- 📎 **Official Python guide to private variables and name mangling** — https://docs.python.org/3/tutorial/classes.html#private-variables

The `Student` class now controls its own data. A college also has teachers, and teachers share common details with students, such as a name. In the next topic, **Inheritance**, you'll learn how one class can reuse the attributes and methods of another.
