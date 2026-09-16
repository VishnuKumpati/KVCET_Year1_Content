# Introduction to Object-Oriented Programming

A bank account has data — an owner and a balance — and things that can be done to it. Written with the tools met so far, that is a dictionary and some functions:

```python
account = {"owner": "Anita", "balance": 1000}

def deposit(account, amount):
    account["balance"] = account["balance"] + amount

def withdraw(account, amount):
    if amount > account["balance"]:
        print("Insufficient funds")
        return
    account["balance"] = account["balance"] - amount

deposit(account, 500)
withdraw(account, 200)
print(account["balance"])
```

**Output:**

```
1300
```

This works. It also has four problems, and they get worse as a program grows.

**Nothing connects the data to the functions.** `deposit()` is an ordinary function that happens to expect a dictionary shaped a particular way. Nothing stops it being called with a dictionary describing a student, and nothing tells a reader which functions belong to an account.

**Nothing protects the data.** Every rule in `withdraw()` can be bypassed:

```python
account["balance"] = -99999
print(account["balance"])
```

**Output:**

```
-99999
```

The balance is now impossible, and no function was involved. The checks only apply to code that chooses to go through them.

**Nothing guarantees the shape.** A missing key is discovered when something reads it:

```python
account = {"owner": "Ravi"}
deposit(account, 500)
```

**Output:**

```
KeyError: 'balance'
```

Nothing required an account to have a balance in the first place.

**Typos are silent.** Dictionaries accept any key:

```python
account = {"owner": "Anita", "balance": 1000}
account["balence"] = 5000
print(account)
```

**Output:**

```
{'owner': 'Anita', 'balance': 1000, 'balence': 5000}
```

No error. A new key was created, the real balance is unchanged, and the bug surfaces somewhere else entirely.

## What Object-Oriented Programming Changes

The approach used above is called **procedural programming**: data in one place, functions in another, and the programmer responsible for keeping them in step.

**Object-oriented programming** binds the two together. The data and the operations that are allowed on it become one thing, called an **object**. An account object holds its own balance and knows how to deposit into itself, and code outside it goes through those operations rather than reaching in.

The same account, written this way:

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance = self.balance + amount

    def withdraw(self, amount):
        if amount > self.balance:
            print("Insufficient funds")
            return
        self.balance = self.balance - amount

account = BankAccount("Anita", 1000)
account.deposit(500)
account.withdraw(200)
print(account.balance)
```

**Output:**

```
1300
```

Every part of this is explained over the chapters that follow. What matters now is what changed:

- `deposit` and `withdraw` are written **inside** `BankAccount`, so it is obvious what they belong to.
- An account cannot be created without an owner and a balance. The definition demands both.
- `account.deposit(500)` reads as the account doing something, not as a function being handed a dictionary.
- A misspelled attribute is an error rather than a silently created key.

## You Have Been Using Objects All Along

None of this is new to Python. Every value in the language is already an object:

```python
print(type(5))
print(type("hello"))
print(type([1, 2, 3]))
print(type({"a": 1}))
```

**Output:**

```
<class 'int'>
<class 'str'>
<class 'list'>
<class 'dict'>
```

Every one reports a **class**. That is what `type()` has been saying all along.

And the methods called on them are exactly the arrangement described above — data with operations attached:

```python
name = "anita"
print(name.upper())

marks = [78, 91]
marks.append(64)
print(marks)
```

**Output:**

```
ANITA
[78, 91, 64]
```

`name.upper()` is an operation belonging to the string, called on that string. `marks.append(64)` changes the list it was called on. Neither needed the value passed as an argument, because the value is what the method was called on.

Writing a class is writing a new type that behaves this way. `BankAccount` becomes as much a type as `list` is, and `account.deposit(500)` works for the same reason `marks.append(64)` does.

## The Four Principles

Object-oriented programming is usually described through four ideas. Each is covered properly in its own right; this is the map.

**Encapsulation** — keeping data and the operations on it together, and controlling what outside code may touch. This is what stops a balance being set to `-99999`.

**Inheritance** — building a class from an existing one, keeping what it already does and adding or changing the rest, so shared behaviour is written once.

**Polymorphism** — one operation working across several types, each responding in its own way. `len()` works on a string, a list and a dictionary, and each computes its length differently.

**Abstraction** — exposing what something does while hiding how. Calling `marks.sort()` requires no knowledge of the sorting algorithm.

## When It Is Worth It

A class is worth defining when data and behaviour belong together and the data has rules. An account, a student record with a grade calculation, a shopping cart, a connection that must be opened and closed — each has state that must stay valid, which is what a class protects.

A class is not worth defining for everything. A function that transforms its input and returns a result needs no class. A group of values with no rules and no behaviour is a dictionary or a tuple, and making it a class adds ceremony without adding anything.

The test is whether there is **state that must stay consistent** and **behaviour that depends on it**. When both are present, a class earns its place. When only one is, a function or a dictionary is the simpler answer, and the simpler answer is the better one.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **Object-oriented programming in Python** — https://realpython.com/python3-object-oriented-programming/

Procedural code separates data from the functions that act on it, and nothing keeps them honest. An object binds them together. Next, the syntax that defines one.
