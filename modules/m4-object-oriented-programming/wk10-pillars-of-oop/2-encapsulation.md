# Encapsulation

A `Student` object stores its data in attributes, such as a name and marks. Any part of a program can read or change these attributes directly. This is convenient, but it is also risky. Marks must stay between 0 and 100, yet nothing stops a line of code from setting them to `150` or `-10`.

To address this, a class needs a way to control how its data is accessed and changed. Object-oriented programming provides this through **encapsulation**, one of the four pillars of OOP.

> **Encapsulation combines an object's data and the methods that work on that data within a class, while controlling how the data is accessed and changed. This helps protect the data from invalid changes.**

## The Problem Without Encapsulation

In the class below, the marks can be changed directly from outside the class.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student1.marks = 150

print(student1.marks)
```

**Output:**

```
150
```

Marks above 100 are impossible, but Python stores `150` because nothing in the class checks the new value. Encapsulation helps address this problem by controlling how data is accessed and changed. A class can use methods to **validate new values** before storing them.

## Access Levels in Python

Some programming languages use keywords to restrict access to data. Python does not. Instead, **the way an attribute's name begins** shows how it is meant to be used. In the table, `attribute` stands for any attribute name.

| Access Level | Name Begins With | Meaning |
| --- | --- | --- |
| **Public** | `attribute` | Can be used from anywhere in the program. |
| **Protected** | `_attribute` | Intended for internal use within the class and its child classes. Python does not block outside access. |
| **Private** | `__attribute` | Cannot normally be used by that name from outside the class. |

The same naming rules apply to methods. The `name` and `marks` attributes in the previous example are public. A **child class** is a class that inherits from another class, which is covered in **Inheritance**.

## Protected Attributes

One leading underscore marks an attribute as **protected** by convention. It signals that the attribute is intended for internal use within the class and its child classes.

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

The value is printed, because Python does **not** block access to `_marks`. The underscore is only a **naming convention**. According to Python standards, code outside the class should not use a protected attribute directly.

## Private Attributes

Two leading underscores trigger **name mangling** for an attribute, which makes it **private** by convention. The attribute can still be used normally inside the class.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = marks

    def show_marks(self):
        print("Marks:", self.__marks)

student1 = Student("Ravi", 85)
student1.show_marks()
```

**Output:**

```
Marks: 85
```

`show_marks()` can access `self.__marks` inside the class. However, `student1.__marks` raises an **`AttributeError`** outside the class, because Python changes the attribute's name through name mangling.

## Name Mangling

A private attribute cannot be found from outside the class because Python changes its name.

> **Name mangling is the process in which Python renames an attribute that starts with two underscores by adding an underscore and the class name before it.**

| Name Written in the Class | Name Python Stores |
| --- | --- |
| `__attribute` | `_ClassName__attribute` |
| `__marks` in the `Student` class | `_Student__marks` |

This is why `student1.__marks` raises an `AttributeError` outside the class. The mangled name, `student1._Student__marks`, would still give `85`, so the attribute is **not completely hidden**. Name mangling helps prevent accidental name conflicts and access, but it does not prevent deliberate access. **Avoid using the mangled name directly in your programs.**

Name mangling does not apply to names that also **end** with two underscores, such as `__init__`.

## Getter and Setter Methods

Once direct access is restricted, the class provides methods to work with the data:

- A **getter** is a method that returns the value of an attribute.
- A **setter** is a method that changes the value of an attribute, usually after checking that the new value is valid.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.__marks = 0
        self.set_marks(marks)

    def get_marks(self):
        return self.__marks

    def set_marks(self, marks):
        if 0 <= marks <= 100:
            self.__marks = marks
        else:
            print("Invalid marks:", marks)

student1 = Student("Ravi", 85)
print(student1.get_marks())

student1.set_marks(150)
print(student1.get_marks())

student1.set_marks(92)
print(student1.get_marks())
```

**Output:**

```
85
Invalid marks: 150
85
92
```

- **`set_marks()`** accepts the new marks only if they are between 0 and 100. Otherwise, it prints a message and **keeps the old marks**.
- **`__init__()`** first sets the marks to `0`, then calls `set_marks()`. The same validation therefore runs when an object is created. If `Student("Ravi", 150)` is used, the setter prints a message and leaves the marks at `0`.
- **`get_marks()`** returns the marks, so other code can read them without touching the private attribute.

The checking rule is written **once**, inside `set_marks()`. The class now decides how its own data can be changed. The names `get_marks()` and `set_marks()` follow a common naming convention; Python does not require these exact names.

Not every attribute needs a getter and a setter. Public attributes are suitable when no special control is needed.

## Points to Remember

- **One leading underscore** (`_attribute`) is a convention for internal use. Python does not block access.
- **Two leading underscores** (`__attribute`) trigger **name mangling**, which helps prevent accidental access and naming conflicts but does not make an attribute completely private.
- **A getter** reads a value, and **a setter** changes it after checking it.
- **Encapsulation helps keep data valid**, because changes are made through the class's own methods.

## Further Reading

- 📎 **Encapsulation in Python** — https://www.tutorialspoint.com/python/python_encapsulation.htm
- 📎 **Official Python guide to private variables and name mangling** — https://docs.python.org/3/tutorial/classes.html#private-variables

A `Teacher` also has a name and needs to introduce themselves, just like a `Student`. Writing the same code in both classes would repeat it. In the next topic, **Inheritance**, you'll learn how one class can reuse the attributes and methods of another.
