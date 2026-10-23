# Polymorphism

A program may work with different objects that perform the same action in different ways. **Polymorphism** lets us write code that works with these objects without writing separate code for each one.

> **Polymorphism means the same operation can behave differently for different kinds of objects or values.**

The word comes from Greek: **poly** means "many" and **morph** means "form". So polymorphism means "many forms".

## How Python Supports Polymorphism

Python allows the same operation to work with different objects or values in several ways:

- **Method overriding:** Child classes provide their own versions of an inherited method, allowing the same method call to behave differently for different objects.
- **Built-in functions:** A function such as `len()` works with different types of objects.
- **Operators:** An operator such as `+` performs different operations depending on the values.
- **Duck typing:** A function works with different objects as long as they provide the methods it needs.

The examples below show how each approach works in Python.

## Polymorphism with Method Overriding

In **Inheritance**, you learned that a child class can override a method inherited from its parent class. When different child objects use the same method call, each object can produce different behaviour.

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self):
        print("Woof!")

class Cat(Animal):
    def speak(self):
        print("Meow!")

animals = [Dog(), Cat()]

for animal in animals:
    animal.speak()
```

**Output:**

```
Woof!
Meow!
```

Both `Dog` and `Cat` inherit from `Animal`, but each overrides `speak()` with its own behaviour. The loop does not check which kind of animal it has. It writes one call, `animal.speak()`, and each object runs its own version.

Python chooses which version to run based on the object's class when the method is called. Because this choice is made while the program is running, this is called **runtime polymorphism**.

Overriding is used whenever related classes share an action but perform it differently.

| Parent Class | Child Classes | Overridden Method |
| --- | --- | --- |
| `Account` | `SavingsAccount`, `CurrentAccount` | `calculate_interest()`, because each account type earns interest differently |
| `Shape` | `Circle`, `Rectangle` | `area()`, because each shape uses a different formula |
| `Payment` | `UPIPayment`, `CardPayment` | `pay()`, because each payment method works differently |

## Polymorphism in Built-in Functions

Many built-in functions work with different types of values.

```python
print(len("Python"))
print(len([10, 20, 30]))
print(len({"name": "Ravi", "marks": 85}))
```

**Output:**

```
6
3
2
```

The same function, `len()`, behaves differently for each type:

- For a **string**, it returns the number of characters.
- For a **list**, it returns the number of items.
- For a **dictionary**, it returns the number of keys.

## Polymorphism in Operators

The same operator can also behave differently for different types of values.

```python
print(10 + 5)
print("Py" + "thon")
print([1, 2] + [3, 4])

print(3 * 4)
print("Ha" * 3)
print([0] * 3)
```

**Output:**

```
15
Python
[1, 2, 3, 4]
12
HaHaHa
[0, 0, 0]
```

| Operator | With Numbers | With Strings | With Lists |
| --- | --- | --- | --- |
| `+` | Adds the numbers | Joins the strings | Joins the lists |
| `*` | Multiplies the numbers | Repeats the string the given number of times | Repeats the list the given number of times |

With strings and lists, `*` is used with an integer, such as `"Ha" * 3`.

Python also lets you decide how operators such as `+` behave for objects of your own classes. This is called **operator overloading**, and it is covered in **Dunder Methods**.

## Polymorphism with Duck Typing

Classes do not need a shared parent class to have methods with the **same name**. A single function can call that method on objects of any of these classes. This is called **duck typing**.

> **Duck typing means an object can be used as long as it supports the methods the code needs, regardless of its class.**

The name comes from a well-known saying: if something walks like a duck and quacks like a duck, it can be treated as a duck.

```python
class UPIPayment:
    def pay(self, amount):
        print("Paid", amount, "using UPI")

class CardPayment:
    def pay(self, amount):
        print("Paid", amount, "using a card")

def make_payment(payment, amount):
    payment.pay(amount)

make_payment(UPIPayment(), 500)
make_payment(CardPayment(), 1200)
```

**Output:**

```
Paid 500 using UPI
Paid 1200 using a card
```

`UPIPayment` and `CardPayment` are **separate classes**. Neither inherits from a shared payment class, yet `make_payment()` works with both, because both have a `pay()` method.

If an object has no `pay` attribute, calling `make_payment()` with it raises an **`AttributeError`**.

## Method Overloading

> **Method overloading means defining two or more methods in the same class with the same name but a different number or type of parameters.**

Languages such as Java support method overloading. **Python does not support method overloading in the traditional way.** If a class defines two methods with the same name, the later definition replaces the earlier one.

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

The second `add()` replaces the first. Calling `calc.add(1, 2)` raises a **`TypeError`** because the remaining method requires three arguments (`a`, `b` and `c`), but only two are passed. Python passes `self` automatically when the method is called on an object.

## Accepting Different Arguments in Python

Although Python does not support traditional method overloading, a method can be designed to accept different arguments. Python provides three useful techniques.

### Using Default Arguments

> **A default argument gives a parameter a default value. Python uses this value when the caller does not pass that argument.**

```python
class Greeting:
    def say_hello(self, name="Guest"):
        print("Hello,", name)

greeting = Greeting()
greeting.say_hello()
greeting.say_hello("Ravi")
```

**Output:**

```
Hello, Guest
Hello, Ravi
```

When no name is passed, `name` uses its default value, `"Guest"`. When `"Ravi"` is passed, it replaces the default value.

### Accepting Any Number of Positional Arguments with `*args`

> **`*args` collects extra positional arguments into a tuple.**

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

The `*` before `numbers` collects all the values passed into one tuple, such as `(10, 20, 30)`. The same `add()` method works with two, three or four numbers. The name `args` is only a convention, so any name can be used after the `*`.

### Accepting Any Number of Keyword Arguments with `**kwargs`

> **`**kwargs` collects extra keyword arguments into a dictionary.**

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

The `**` before `details` collects all the keyword arguments into one dictionary, such as `{"name": "Ravi", "marks": 85}`. The same method works with two details or three. The name `kwargs` is also only a convention, so any name can be used after the `**`.

**These techniques make methods flexible, but they are not method overloading.** They do not create multiple method definitions or automatically choose a different version based on the number or types of the arguments.

## Further Reading

- 📎 **Polymorphism in Python with examples** — https://www.tutorialspoint.com/python/python_polymorphism.htm
- 📎 **Official Python guide to default arguments, `*args` and `**kwargs`** — https://docs.python.org/3/tutorial/controlflow.html#default-argument-values
- 📎 **Official Python glossary entry for duck typing** — https://docs.python.org/3/glossary.html#term-duck-typing

Sometimes, we want to ensure that every child class provides a required method. In the next topic, **Abstraction**, the fourth pillar, you'll learn how abstract base classes help us do this.
