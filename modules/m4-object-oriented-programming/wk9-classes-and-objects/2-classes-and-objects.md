# Classes and Objects

Two words are used constantly and mean different things.

A **class** is a definition: what this kind of thing has, and what it can do. It describes, and it holds no data of its own.

An **object** is one actual thing built from that definition. It has its own data. An object is also called an **instance** of its class.

One class, any number of objects. `BankAccount` is written once; a bank has millions of accounts, each with its own owner and balance, all built from that one definition.

## Defining a Class

The `class` keyword, a name, a colon, and an indented body:

```
class ClassName:
    body
```

The body cannot be empty, so a class with nothing in it yet needs `pass`:

```python
class Dog:
    pass
```

That is a complete, working class. It defines a new type that holds nothing and does nothing, which is enough to demonstrate what a class and an object are.

Class names are written in **PascalCase** — each word capitalised, no underscores:

```python
class BankAccount:
    pass

class Student:
    pass
```

This is not enforced, and it matters anyway. Functions and variables use `snake_case`, so the capital letter is how a reader tells `BankAccount` (a class) from `bank_account` (a variable holding one). Follow it.

## Creating an Object

Call the class as though it were a function:

```python
class Dog:
    pass

first = Dog()
```

`Dog()` builds a new object and hands it back. `first` now refers to one Dog.

The brackets are what makes the difference. `Dog` is the class itself; `Dog()` is a new object built from it:

```python
class Dog:
    pass

print(type(Dog))
print(type(Dog()))
```

**Output:**

```
<class 'type'>
<class '__main__.Dog'>
```

The class itself is an object too — an object of type `type`. That is a fact worth knowing and not one to dwell on.

`__main__` in the second line is the name Python gives to the file being run directly. A class's full name includes where it was defined, so a `Dog` defined in the file you ran is `__main__.Dog`. The part that matters is `Dog`.

## Every Object Is Separate

Each call to the class produces a new object:

```python
class Dog:
    pass

first = Dog()
second = Dog()

print(first is second)
print(first == second)
```

**Output:**

```
False
False
```

Two separate objects. `is` reports that they are different objects, as expected. `==` also reports `False`, which is worth noting: by default, two objects of a class you define are equal only if they are the *same* object. Comparing their contents is behaviour a class has to be given.

Assignment does not copy an object, exactly as it does not copy a list:

```python
class Dog:
    pass

first = Dog()
third = first

print(first is third)
```

**Output:**

```
True
```

One object, two names.

## Checking the Type

`type()` reports an object's class:

```python
class Dog:
    pass

class Cat:
    pass

pet = Dog()
print(type(pet))
```

**Output:**

```
<class '__main__.Dog'>
```

For testing rather than displaying, `isinstance()` is the right tool. It takes an object and a class and returns `True` or `False`:

```python
class Dog:
    pass

class Cat:
    pass

pet = Dog()
print(isinstance(pet, Dog))
print(isinstance(pet, Cat))
```

**Output:**

```
True
False
```

`isinstance()` works on built-in types too, which is the usual way to check what something is:

```python
print(isinstance(5, int))
print(isinstance("hello", str))
print(isinstance([1, 2], list))
print(isinstance(5, str))
```

**Output:**

```
True
True
True
False
```

Prefer `isinstance(value, int)` to `type(value) == int`. The two agree here, and `isinstance()` behaves correctly in cases the next week introduces, where one class is built from another.

## A Class with Something in It

A class body can contain function definitions, written with `def` as usual:

```python
class Dog:
    def bark(self):
        print("Woof")

pet = Dog()
pet.bark()
```

**Output:**

```
Woof
```

A function defined inside a class is called a **method**, and it is called on an object with a dot. `pet.bark()` is the same shape as `marks.append(64)` — an operation belonging to a value, called on that value.

The `self` parameter appears in every method and is the subject of its own chapter. For now: it is required, and nothing is passed for it at the call.

Two objects of the same class share the definition, so both can do the same things:

```python
class Dog:
    def bark(self):
        print("Woof")

first = Dog()
second = Dog()
first.bark()
second.bark()
```

**Output:**

```
Woof
Woof
```

One `bark` was written. Both objects have it, because both were built from the same class.

## Class Against Object

| | Class | Object |
| --- | --- | --- |
| What it is | a definition | one thing built from it |
| Written with | `class Name:` | `Name()` |
| How many | one | as many as needed |
| Holds data | no | yes, its own |
| Also called | type | instance |

The blueprint comparison is the usual one and it is reasonably accurate: a blueprint describes a house, is drawn once, and any number of houses are built from it, each with its own address and its own occupants. The blueprint is not a house, and you cannot live in it.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **Classes and objects explained** — https://www.programiz.com/python-programming/class

A class defines a type, and calling it produces an object of that type. Next, giving those objects data of their own.
