# Methods

A `Student` object can now hold its own name and marks. A college application also needs to perform actions on that data, such as displaying a student's details, adding bonus marks, or checking whether a student has passed. In Python, these actions are defined as **methods**.

## Functions and Methods

A standalone **function** can receive a student's name and marks as arguments. Each time you call it, you pass the values it needs.

```python
def display_details(name, marks):
    print(name, marks)

display_details("Ravi", 85)
```

**Output:**

```
Ravi 85
```

A method takes a different approach.

> **A method is a function defined inside a class. An instance method can work with the data of the object it is called on.**

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def display_details(self):
        print(self.name, self.marks)
```

This code only defines the class, so running it produces no output. Here, `display_details` is defined inside the `Student` class. It does not need the name and marks passed in as arguments because it reads them from the object through **`self`**. A method can still accept additional arguments when needed, as shown in **Methods with Parameters**.

| | Function | Instance Method |
| --- | --- | --- |
| Defined | Outside a class | Inside a class |
| Accesses student data | Through arguments or other available variables | Through `self`, and can also accept additional arguments |

All the methods in this topic are **instance methods**.

## Defining and Calling a Method

A method is defined inside the class using the **`def`** keyword. Its first parameter is `self`, and any other parameters come after it.

**Syntax:**

```python
class ClassName:
    def method_name(self, parameter1):
        pass
```

According to Python standards, method names are written in **lowercase words separated by underscores**, such as `display_details` or `add_bonus`.

A method is called using **dot notation**: the object name, a dot, the method name, and **parentheses**. The following example calls the same method on two objects to show how an instance method accesses each object's existing data.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def display_details(self):
        print(self.name, self.marks)

student1 = Student("Ravi", 85)
student2 = Student("Priya", 92)

student1.display_details()
student2.display_details()
```

**Output:**

```
Ravi 85
Priya 92
```

When you call `student1.display_details()`, Python passes `student1` automatically as `self`, allowing the method to access that student's attributes. The same method therefore displays each student's own details.

## Methods with Parameters

A method can take parameters after `self`. Their values are written inside the parentheses when the method is called.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def add_bonus(self, bonus):
        self.marks = self.marks + bonus

student1 = Student("Ravi", 85)
student1.add_bonus(5)

print(student1.marks)
```

**Output:**

```
90
```

In `student1.add_bonus(5)`, the parameter `bonus` receives `5`, and the method adds it to Ravi's marks. This shows that **a method can change the data of the object it is called on**.

## Methods That Return a Value

A method can send a result back to the caller using the **`return`** statement, just like a function. This example uses a pass mark of 40.

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def has_passed(self):
        return self.marks >= 40

student1 = Student("Ravi", 85)
student2 = Student("Arun", 32)

print(student1.has_passed())
print(student2.has_passed())
```

**Output:**

```
True
False
```

`has_passed()` compares the object's own marks with 40 and returns the result. Ravi's marks are 85, so the result is `True`. Arun's marks are 32, so the result is `False`.

The `Student` class now keeps its data and the actions that work on that data **together in one place**, which is a key idea of object-oriented programming.

## Further Reading

- 📎 **Official Python guide to method objects** — https://docs.python.org/3/tutorial/classes.html#method-objects
- 📎 **Python standard naming convention for methods** — https://peps.python.org/pep-0008/#method-names-and-instance-variables

Every student object has its own `name` and `marks`, but the college name is the same for every student. Storing that same value separately in every object would repeat it many times. In the next topic, **Class Attributes and Class Methods**, you'll learn how to define data shared by all objects of a class.
