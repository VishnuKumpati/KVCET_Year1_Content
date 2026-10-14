# Attributes and the `__init__` Method

A `Student` object created from an empty class exists, but it holds no student details. A college application needs every student object to carry its own information, such as a name and marks. In Python, this information is stored as **attributes**.

## Attributes

> **An attribute is a piece of data that belongs to an object.**

An attribute that belongs to one particular object is called an **instance attribute**. Each object can have its **own value** for that attribute. For example, one `Student` object can have the name `"Ravi"` while another has the name `"Priya"`.

## The `__init__` Method

To give every new `Student` object its own name and marks, Python provides a special method called **`__init__`**.

> **`__init__` is a special method Python calls automatically to initialise a new object. It sets up the object's starting attributes.**

A **method** is a function defined inside a class. The name `__init__` has **two underscores** on each side, and it is associated with initialisation.

**Syntax:**

```python
class ClassName:
    def __init__(self, parameter1, parameter2):
        self.attribute1 = parameter1
        self.attribute2 = parameter2
```

| Part | Purpose |
| --- | --- |
| `def __init__` | Defines the `__init__` method inside the class. |
| `self` | The first parameter, which refers to the new object. |
| `parameter1`, `parameter2` | Receive the values passed when the object is created. |
| `self.attribute1 = parameter1` | Stores the parameter's value in an attribute of the object. |

Python creates the object and then calls `__init__` to initialise it. You may also hear `__init__` called a **constructor**.

## The `self` Parameter

> **`self` is the first parameter of `__init__`, and it refers to the new object being initialised.**

The statement `self.name = name` has two sides:

- **`self.name`** on the left refers to the object's `name` attribute. If it doesn't exist yet, the assignment creates it.
- **`name`** on the right is the parameter that holds the value passed in.

**Python passes `self` automatically.** It is never written inside the parentheses when an object is created.

The name `self` is a **convention, not a keyword**. Python accepts another name, but according to Python standards, `self` should always be used.

## Creating Objects with Attributes

The values for the attributes are written inside the parentheses when the object is created.

When you create `Student("Ravi", 85)`, the values `"Ravi"` and `85` are passed to `__init__`, which stores them in the new object's attributes.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

print(student1.name, student1.marks)
print(student2.name, student2.marks)
```

**Output:**

```
Ravi 85
Priya 92
```

An attribute is read using **dot notation**: the object name, a dot, and the attribute name. `student1.name` reads the `name` attribute of `student1`.

Both `name` and `marks` are **required arguments**. If you call `Student("Ravi")`, Python raises a **`TypeError`** because the `marks` argument is missing.

## Updating an Attribute

An attribute's value can be changed by assigning a new value to it with dot notation.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

student1.marks = 90

print(student1.marks)
print(student2.marks)
```

**Output:**

```
90
92
```

Only `student1` changes. **Updating an instance attribute on one object does not affect any other object**, because each object stores its own value.

## Further Reading

- 📎 **Official Python guide to class objects and `__init__`** — https://docs.python.org/3/tutorial/classes.html#class-objects
- 📎 **Official Python guide to instance objects** — https://docs.python.org/3/tutorial/classes.html#instance-objects

Each `Student` object now holds its own name and marks. In the next topic, **Methods**, you'll learn how to give objects behaviour, such as adding bonus marks to a student.
