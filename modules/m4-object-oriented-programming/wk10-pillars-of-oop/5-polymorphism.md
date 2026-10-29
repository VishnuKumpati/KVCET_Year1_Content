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

### Why Python Replaces the Earlier Method

There are two reasons.

**1. `def` assigns a function to a name.** In Python, `def` creates a function object and **binds** it to a name, much like `=` assigns a value to a variable. Defining a function with the same name again binds that name to the new function, and the earlier function is no longer reachable through it.

```python
def greet():
    print("First version")

def greet():
    print("Second version")

greet()
```

**Output:**

```
Second version
```

This works exactly like `x = 1` followed by `x = 2`: the name `x` keeps only the latest value. Inside a class, each method name is stored in the class in the same way, so the second `add()` replaces the first.

**2. Python does not choose a method by its parameter types.** In Java, every parameter has a declared type, such as `int` or `double`, and Java picks the matching method before the program runs. Python is **dynamically typed**: parameters have no declared types, and Python finds a method **only by its name** when it is called. With only one method per name, Python cannot choose between versions by the number or types of arguments.

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

## Rules for Using `*args` and `**kwargs`

### The Order of Parameters

When a function or method combines different kinds of parameters, they **must** appear in this order:

| Order | Kind of Parameter | Example |
| --- | --- | --- |
| 1 | Regular parameters | `name` |
| 2 | Parameters with default values | `greeting="Hello"` |
| 3 | `*args` | `*others` |
| 4 | Keyword-only parameters, which come after `*args` | `city="Chennai"` |
| 5 | `**kwargs` | `**details` |

Not every function needs all five. Whichever ones are used must keep this order.

```python
def show(name, greeting="Hello", *others, **details):
    print("name:", name)
    print("greeting:", greeting)
    print("others:", others)
    print("details:", details)

show("Ravi")
print()
show("Ravi", "Hi", "Priya", "Arun", city="Chennai", marks=85)
```

**Output:**

```
name: Ravi
greeting: Hello
others: ()
details: {}

name: Ravi
greeting: Hi
others: ('Priya', 'Arun')
details: {'city': 'Chennai', 'marks': 85}
```

- In the first call, only `name` is passed. `greeting` uses its default, and `others` and `details` are **empty**: an empty tuple, `()`, and an empty dictionary, `{}`.
- In the second call, positional arguments fill `name` and `greeting` **first**. The remaining positional arguments, `"Priya"` and `"Arun"`, go into `others`. The keyword arguments go into `details`.

### Other Rules

- **`**kwargs` must be the last parameter.** Writing `def f(**kwargs, *args):` raises a **`SyntaxError`**.
- **Only one `*` parameter and one `**` parameter are allowed** in a function.
- **`*args` is a tuple and `**kwargs` is a dictionary**, so they are used like any tuple or dictionary.
- **The names `args` and `kwargs` are conventions.** The `*` and `**` symbols are what matter.
- **When calling a function, positional arguments must come before keyword arguments.** `f(a=1, 2)` raises a **`SyntaxError`**.

## Edge Cases

### A Default Parameter Before `*args` Can Be Filled by Mistake

Positional arguments always fill regular and default parameters **before** `*args`. So a default parameter placed before `*args` can receive a value that was meant for `*args`.

```python
def welcome(greeting="Hello", *names):
    for name in names:
        print(greeting, name)

welcome("Ravi", "Priya", "Arun")
```

**Output:**

```
Ravi Priya
Ravi Arun
```

`"Ravi"` was meant to be a name, but it filled `greeting`, because `greeting` comes first. Only `"Priya"` and `"Arun"` went into `names`.

**The fix is to place the default parameter after `*args`.** It then becomes **keyword-only**, so it can be changed only by name:

```python
def welcome(*names, greeting="Hello"):
    for name in names:
        print(greeting, name)

welcome("Ravi", "Priya", "Arun")
welcome("Ravi", "Priya", greeting="Hi")
```

**Output:**

```
Hello Ravi
Hello Priya
Hello Arun
Hi Ravi
Hi Priya
```

- All positional arguments now go into `names`.
- `greeting` keeps its default unless it is passed by name, as in `greeting="Hi"`.

### A Default Parameter with `**kwargs`

A keyword argument whose name matches a parameter goes to **that parameter**, not into `**kwargs`. Only keyword arguments with **no matching parameter** are collected into `**kwargs`.

```python
def register(name, city="Chennai", **details):
    print("city:", city)
    print("details:", details)

register("Ravi", city="Madurai", marks=85)
```

**Output:**

```
city: Madurai
details: {'marks': 85}
```

- `city="Madurai"` matches the parameter `city`, so it replaces the default value, `"Chennai"`.
- `marks=85` matches no parameter, so it is collected into `details`.

### The Same Argument Passed Twice

A parameter can receive only **one** value. Calling `register("Ravi", name="Priya")` passes `name` both by position and by keyword, so Python raises a **`TypeError`**: `register() got multiple values for argument 'name'`.

## Points to Remember

- **Polymorphism** lets the same method or function behave differently depending on the object.
- **Method overriding** achieves polymorphism by letting child classes provide their own version of an inherited method.
- **Python does not support traditional method overloading.** A later method with the same name replaces the earlier one.
- **`*args` and `**kwargs`** let one method accept a different number of arguments.
- **Parameter order** is regular parameters, default parameters, `*args`, keyword-only parameters, then `**kwargs`.

## Further Reading

- 📎 **Polymorphism in Python** — https://www.tutorialspoint.com/python/python_polymorphism.htm
- 📎 **Official Python tutorial: how child classes override methods** — https://docs.python.org/3/tutorial/classes.html#inheritance
- 📎 **Official Python guide to arbitrary argument lists (`*args`)** — https://docs.python.org/3/tutorial/controlflow.html#arbitrary-argument-lists
- 📎 **Official Python guide to keyword arguments (`**kwargs`)** — https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments

Sometimes, a parent class must require its concrete child classes to implement specific methods. In the next topic, **Abstraction**, you'll learn how abstract classes help enforce this requirement.
