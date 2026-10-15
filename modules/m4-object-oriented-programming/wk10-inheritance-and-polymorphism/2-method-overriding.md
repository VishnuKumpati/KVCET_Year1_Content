# Method Overriding

A child class inherits every method of its parent exactly as it is written. Sometimes an inherited method does not suit the child. A student's name should be displayed as `Student: Ravi`, not `Name: Ravi`.

## Method Overriding

> **Method overriding means defining a method in a child class with the same name as a method in its parent class. The child's version replaces the parent's version for objects of the child class.**

No special keyword is needed. Writing a method with the same name as the parent's method is enough.

## Overriding a Method

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def display_name(self):
        print("Student:", self.name)

class Teacher(Person):
    pass

student1 = Student("Ravi")
teacher1 = Teacher("Meena")

student1.display_name()
teacher1.display_name()
```

**Output:**

```
Student: Ravi
Name: Meena
```

`Student` defines its own `display_name()`, so `student1` uses that version. `Teacher` does not, so `teacher1` uses the version inherited from `Person`.

Python checks the object's own class first and stops at the first match. For `student1.display_name()`, the method is found in `Student`, so the search never reaches `Person`. The parent's method still exists and is still used by `Person` and `Teacher` objects.

## Overriding the __init__ Method

`__init__` is a method, so a child class can override it too. This is needed when a child has extra attributes, such as a student's course.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, course):
        self.course = course

student1 = Student("Ravi", "Python")
print(student1.course)
student1.display_name()
```

**Output:**

```
Python
AttributeError: 'Student' object has no attribute 'name'
```

`Student`'s `__init__` replaced `Person`'s `__init__` completely. The line `self.name = name` in `Person` never ran, so the object has a `course` attribute but no `name` attribute. The inherited `display_name()` method then fails, because it uses `self.name`.

One fix is to write `self.name = name` again in `Student`. That works, but it repeats the code that inheritance was meant to share. The correct fix is to run the parent's `__init__` method from inside the child's.

## Further Reading

- 📎 **Official Python guide to inheritance** — https://docs.python.org/3/tutorial/classes.html#inheritance

A child class can replace any inherited method by defining one with the same name. When the child replaces `__init__`, the parent's setup is lost. In the next topic, **The super() Function**, you'll learn how a child class can call its parent's methods.
