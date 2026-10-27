# Polymorphism

One person can play different roles. At home, they are a family member. At college, they may be a student or a teacher. The person is the same, but their behaviour changes with the role.

Classes can work in a similar way. A `Person` class might define an `introduce()` method, while its child classes, `Student` and `Teacher`, provide their own versions. The same method call, `introduce()`, can then produce different results depending on the object. This is **polymorphism**, one of the four pillars of OOP.

> **Polymorphism allows the same method call or operation to behave differently depending on the object or value involved.**

The word comes from Greek: **poly** means "many" and **morph** means "form". So polymorphism means "many forms".

## Ways to Achieve Polymorphism

In object-oriented programming, polymorphism is commonly achieved in two ways:

| Way | What It Means | In This Course |
| --- | --- | --- |
| **Method overriding** | A child class provides its own version of a method it inherits, using the same name | Covered in **Method Overriding and `super()`** |
| **Method overloading** | One class has several methods with the same name, but with different parameters | Explained below |

Method overriding has already shown polymorphism in action: the same method call gives different results for a parent object and a child object. This topic explains method overloading and how Python handles it.

## Method Overloading

> **Method overloading means defining two or more methods in the same class with the same name, but with a different number or type of parameters.**

Languages such as Java support method overloading. A Java class can have one `add()` method that takes two numbers and another `add()` method that takes three numbers. Java chooses the right one based on how many arguments are passed.

**Python does not support method overloading in this traditional way.** In a Python class, defining a method again with the same name **replaces the earlier definition** under that name. Only the last definition remains available through that name.

```python
class Calculator:
    def add(self, a, b):
        return a + b

    def add(self, a, b, c):
        return a + b + c

calc = Calculator()
print(calc.add(1, 2, 3))
```

**Output:**

```
6
```

- Both methods are named `add()`, so the second definition **replaces** the first.
- Only the three-number version remains, so `calc.add(1, 2, 3)` works.
- `calc.add(1, 2)` would raise a **`TypeError`**, because the remaining `add()` needs three numbers but receives only two.

## Accepting Different Arguments with `*args` and `**kwargs`

Python can handle different arguments through a single method instead of defining multiple methods with the same name. Two useful tools for this are `*args` and `**kwargs`.

### Using `*args`

> **`*args` collects positional arguments into a tuple, allowing a function or method to accept a variable number of arguments.**

A **positional argument** is an argument passed without a name, such as `10`. Python matches it to a parameter by its position.

```python
class Calculator:
    def add(self, *numbers):
        return sum(numbers)

calc = Calculator()
print(calc.add(10, 20))
print(calc.add(10, 20, 30))
print(calc.add(10, 20, 30, 40))
```

**Output:**

```
30
60
100
```

- The `*` before `numbers` collects all the numbers passed into a tuple. For example, `calc.add(10, 20, 30)` makes `numbers` equal to `(10, 20, 30)`.
- `sum(numbers)` adds all the values in the tuple.
- The **same** `add()` method works with two, three or four numbers.

The name `args` is only a convention, so any name can be used after the `*`.

### Using `**kwargs`

> **`**kwargs` collects keyword arguments into a dictionary, allowing a function or method to accept any number of them.**

A **keyword argument** is an argument passed with a name, such as `name="Ravi"`.

```python
class Student:
    def show_details(self, **details):
        for key, value in details.items():
            print(key, ":", value)

student1 = Student()
student1.show_details(name="Ravi", marks=85)
print()
student1.show_details(name="Priya", marks=92, city="Chennai")
```

**Output:**

```
name : Ravi
marks : 85

name : Priya
marks : 92
city : Chennai
```

- The `**` before `details` collects all the keyword arguments into a dictionary. For example, the first call makes `details` equal to `{"name": "Ravi", "marks": 85}`.
- The loop prints each name and value from the dictionary.
- The **same** `show_details()` method works with two details or three.

The name `kwargs` is also only a convention, so any name can be used after the `**`.

`*args` and `**kwargs` make one method flexible. They are **not** method overloading, because there is still only one method with that name.

## Points to Remember

- **Polymorphism** lets the same method or function behave differently depending on the object.
- **Method overriding** achieves polymorphism by letting child classes provide their own version of an inherited method.
- **Python does not support traditional method overloading.** A later method with the same name replaces the earlier one.
- **`*args` and `**kwargs`** let one method accept a different number of arguments.

## Further Reading

- 📎 **Polymorphism in Python** — https://www.tutorialspoint.com/python/python_polymorphism.htm
- 📎 **Official Python tutorial: how child classes override methods** — https://docs.python.org/3/tutorial/classes.html#inheritance
- 📎 **Official Python guide to arbitrary argument lists (`*args`)** — https://docs.python.org/3/tutorial/controlflow.html#arbitrary-argument-lists
- 📎 **Official Python guide to keyword arguments (`**kwargs`)** — https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments

Sometimes, a parent class must require its concrete child classes to implement specific methods. In the next topic, **Abstraction**, you'll learn how abstract classes help enforce this requirement.
