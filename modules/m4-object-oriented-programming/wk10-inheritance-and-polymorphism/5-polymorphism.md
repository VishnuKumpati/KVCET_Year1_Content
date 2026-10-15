# Polymorphism

The built-in function `len()` works on a string, a list and a dictionary. Each type counts something different, but the same function name is used for all of them.

```python
print(len("Ravi"))
print(len([87, 72, 95]))
print(len({"Ravi": 85, "Priya": 92}))
```

**Output:**

```
4
3
2
```

`len()` counts characters in a string, elements in a list and key-value pairs in a dictionary. This idea is called **polymorphism**.

## Polymorphism

> **Polymorphism means the same method name behaves differently depending on the object it is called on.** The word means "many forms".

The code that calls the method does not need to know which class the object belongs to. Each object responds in its own way.

## Polymorphism with Inheritance

Child classes that override the same parent method are the most common example of polymorphism.

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
    def display_name(self):
        print("Teacher:", self.name)

people = [Student("Ravi"), Teacher("Meena"), Person("Asha")]

for person in people:
    person.display_name()
```

**Output:**

```
Student: Ravi
Teacher: Meena
Name: Asha
```

The loop calls `person.display_name()` for every object, without checking its class. Each object runs its own version of `display_name()`.

A new child class with its own `display_name()` method can be added to the list, and the loop works without any change.

## Polymorphism with Unrelated Classes

The classes do not need a common parent. Python only checks that the object **has** a method with that name when it is called.

```python
class Student:
    def describe(self):
        print("A student attends classes")

class Library:
    def describe(self):
        print("A library lends books")

for item in [Student(), Library()]:
    item.describe()
```

**Output:**

```
A student attends classes
A library lends books
```

`Student` and `Library` are not related, but both have a `describe` method, so the same loop works on both. This style is called **duck typing**, from the saying "if it walks like a duck and quacks like a duck, it is a duck".

If an object does not have the method, Python raises an `AttributeError` when the method is called.

## Method Overloading in Python

> **Method overloading means defining several methods with the same name in one class, each taking a different number or type of parameters.** Some languages, such as Java and C++, support this.

**Python does not support traditional method overloading.** If a class defines two methods with the same name, the second definition replaces the first.

```python
class Calculator:
    def add(self, a, b):
        return a + b

    def add(self, a, b, c):
        return a + b + c

calc = Calculator()
print(calc.add(10, 20, 30))
print(calc.add(10, 20))
```

**Output:**

```
60
TypeError: Calculator.add() missing 1 required positional argument: 'c'
```

Only the second `add`, with three parameters, exists. The first one was replaced, so calling `add` with two numbers fails.

Python achieves the same result with **default parameter values**. One method handles different numbers of arguments.

```python
class Calculator:
    def add(self, a, b, c=0):
        return a + b + c

calc = Calculator()
print(calc.add(10, 20))
print(calc.add(10, 20, 30))
```

**Output:**

```
30
60
```

When `c` is not passed, it takes its default value `0`. The same method name now works with two or three arguments.

| | Method Overriding | Method Overloading |
| --- | --- | --- |
| Meaning | A child class redefines a method of its parent | One class has several methods with the same name and different parameters |
| Classes involved | Parent and child | One class |
| Supported in Python | Yes | No; default parameter values are used instead |

## Further Reading

- 📎 **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html

Polymorphism works only if every class provides the method being called. Python does not check this in advance. In the next topic, **Abstraction**, you'll learn how a parent class can require its child classes to define certain methods.
