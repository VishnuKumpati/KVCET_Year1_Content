# Class Attributes and Class Methods

Every student in a college has a different name and different marks, but all of them share the same college name. Storing that same college name separately in every `Student` object would repeat it many times. Python solves this with **class attributes**.

## Class Attributes

> **A class attribute is an attribute defined directly inside a class, outside any method. It is shared by all objects of that class.**

The `name` and `marks` attributes are **instance attributes**, because each object has its own value for them. A class attribute is different: it is **stored on the class itself**. Objects can access this shared value unless they have an instance attribute with the same name.

| | Instance Attribute | Class Attribute |
| --- | --- | --- |
| Defined | Usually inside `__init__`, using `self` | Inside the class, outside any method |
| Belongs to | One object | The class, shared by all its objects |
| Example | `name`, `marks` | `college` |

## Defining and Accessing a Class Attribute

A class attribute is written inside the class body, usually at the top, before the methods. It can be accessed through **any object** or through the **class name**.

```python
class Student:
    college = "City College"

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

print(student1.college)
print(student2.college)
print(Student.college)
```

**Output:**

```
City College
City College
City College
```

The value `"City College"` is stored **once**, in the class. Both objects read the same value, and `Student.college` reads it directly from the class.

## Updating a Class Attribute

A class attribute is updated by assigning a new value to it **through the class name**. The change applies to every object, because all of them share the same value.

```python
class Student:
    college = "City College"

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

Student.college = "Metro College"

print(student1.college)
print(student2.college)
```

**Output:**

```
Metro College
Metro College
```

One assignment to `Student.college` updates the college name for both students.

## Class Methods

Some operations concern the whole class rather than one object, such as displaying how many students the college has. These operations are defined as **class methods**.

> **A class method receives the class itself as its first argument, allowing it to work with class-level data and operations.**

**Syntax:**

```python
class ClassName:
    @classmethod
    def method_name(cls):
        pass
```

The line **`@classmethod`**, written directly above a method, tells Python that the method is a class method. This line is called a **decorator**. Decorators are covered in depth in a later topic. For now, use `@classmethod` as fixed syntax.

The first parameter of a class method is **`cls`**, which refers to the **class**. Python passes the class automatically when the method is called. Like `self`, the name `cls` is a convention, not a keyword.

```python
class Student:
    college = "City College"
    total_students = 0

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
        Student.total_students = Student.total_students + 1

    @classmethod
    def display_summary(cls):
        print(cls.college, "has", cls.total_students, "students")

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

Student.display_summary()
```

**Output:**

```
City College has 2 students
```

Each time a `Student` object is created, the last line of `__init__` increases `Student.total_students` by 1. The class name is used here because `__init__` is an instance method: it receives the object as `self`, not the class as `cls`. The class method `display_summary` then reads both class attributes through `cls`. It is called through the class name, because the summary describes the whole class, not one student.

An instance method receives the **object** as `self`, while a class method receives the **class** as `cls`.

**A class method is not required just to change a class attribute.** Direct assignment such as `Student.college = "Metro College"` already does that. A class method is useful when an operation on the class as a whole should be defined inside the class itself.

## Assigning Through an Object

Assigning a value through an object gives a different result. **It does not change the class attribute.** Instead, it creates an instance attribute with the same name on that one object.

```python
class Student:
    college = "City College"

    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

student1.college = "Metro College"

print(student1.college)
print(student2.college)
print(Student.college)
```

**Output:**

```
Metro College
City College
City College
```

When reading `student1.college`, Python first looks for an instance attribute named `college` on the object. `student1` now has one, so Python uses it. `student2` has no such instance attribute, so Python uses the class attribute, which is still `"City College"`.

**To update a class attribute for all objects, assign the new value through the class name**, as shown in **Updating a Class Attribute**.

## Further Reading

- 📎 **Official Python guide to class and instance variables** — https://docs.python.org/3/tutorial/classes.html#class-and-instance-variables
- 📎 **Official Python reference for `classmethod`** — https://docs.python.org/3/library/functions.html#classmethod

Any part of the program can currently change an object's attributes directly, for example by setting `student1.marks` to a negative number. In the next topic, **Encapsulation**, you'll learn how to control access to an object's data.
