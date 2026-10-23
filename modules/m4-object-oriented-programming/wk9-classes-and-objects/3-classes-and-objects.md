# Classes and Objects

A college application has to manage hundreds of students. Every student has the same kind of information, such as a name and marks, so the application needs **one common definition** that applies to all of them.

Think about how a college handles admission forms. The college designs the form format once and uses it for every student. All the forms follow the same format, yet each form is a separate document belonging to a different student.

Python follows the same idea. The common definition is called a **class**, and each separate item created from it is called an **object**.

## Defining a Class

> **A class is a template that defines the data and behaviour associated with its objects.**

A class is created using the **`class`** keyword, followed by the class name and a colon.

```python
class Student:
    pass
```

This code defines a class named `Student`. Running it produces **no output**, because defining a class does not create any object.

**A class body cannot be empty.** The **`pass`** statement is a placeholder that does nothing. It keeps the class valid until real content is added. Without it, Python raises an error because the class body is missing.

### Naming a Class

A class name is an **identifier**, which is a name given to an item in a program, such as a variable, a function or a class. Like every identifier, a class name must follow these key rules:

- It **cannot start with a digit**, so `1Student` is invalid.
- It **cannot contain spaces or hyphens**, so `Student Record` and `Student-Record` are invalid.
- It **cannot be a Python keyword**, such as `class` or `def`.
- It is **case-sensitive**, so `Student` and `student` are two different names.

In addition, according to Python standards, class names follow the **CapWords** convention, where each word starts with a capital letter, such as `Student` and `BankAccount`. This is a convention, not a rule.

## Creating Objects

> **An object is an individual item created from a class, and it is called an instance of that class.**

Creating an object from a class is called **instantiation**. An object is created by writing the class name followed by **parentheses**.

```python
class Student:
    pass

student1 = Student()
student2 = Student()

print(type(student1))
print(student1 is student2)
```

**Output:**

```
<class '__main__.Student'>
False
```

Each call to `Student()` creates a **new instance** of the `Student` class. Assigning it to a variable such as `student1` lets the program refer to that instance later. **The parentheses are required**, because `Student` alone refers to the class itself, not a new instance.

The first output line confirms that `student1` is an instance of the `Student` class. The `__main__` part identifies the module in which the class was defined; you can ignore it for now.

The second output line comes from the **`is`** operator, which checks whether two variables refer to the **same object**. The result is `False` because `student1` and `student2` are two separate objects, even though both were created from the same class.

This is how classes and objects solve the college application problem. The application defines the `Student` class **once** and creates **one object for each student**, so 500 students need only one class and 500 objects.

## Further Reading

- 📎 **Official Python guide to class definition syntax** — https://docs.python.org/3/tutorial/classes.html#class-definition-syntax
- 📎 **Python standard naming convention for classes** — https://peps.python.org/pep-0008/#class-names

The `Student` objects do not hold any student details yet. In the next topic, **Attributes and the `__init__` Method**, you'll learn how to give each object its own name and marks.
