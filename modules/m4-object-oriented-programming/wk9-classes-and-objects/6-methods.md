# Methods

A **method** is a function defined inside a class. It is called on an object, and it can reach that object's data through `self`.

That last part is the whole difference. A plain function receives only what it is given. A method also has the object it was called on, so it can read and change that object's state without being handed it.

## Defining and Calling

Methods are defined with `def` in the class body, and called on an object with a dot:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} says Woof")

    def describe(self):
        print(f"This is {self.name}.")

pet = Dog("Rex")
pet.bark()
pet.describe()
```

**Output:**

```
Rex says Woof
This is Rex.
```

`__init__` is a method too. It is unusual only in being called automatically.

## Parameters Beyond self

After `self`, a method takes parameters like any function — positional, with defaults, or by keyword:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def speak(self, sound="Woof", times=1):
        for count in range(times):
            print(f"{self.name} says {sound}")

pet = Dog("Rex")
pet.speak()
pet.speak("Grrr")
pet.speak("Yip", times=2)
```

**Output:**

```
Rex says Woof
Rex says Grrr
Rex says Yip
Rex says Yip
```

`self` is supplied by the dot; everything after it is supplied by the caller.

## Reading Against Changing

Methods divide into two kinds, and keeping the division clear makes a class much easier to use.

A method that **reads** state and returns something changes nothing:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

shape = Rectangle(4, 5)
print(shape.area())
print(shape.perimeter())
print(shape.width)
```

**Output:**

```
20
18
4
```

Calling `area()` twice gives the same answer, because nothing moved.

A method that **changes** state modifies `self` and usually returns nothing:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def scale(self, factor):
        self.width = self.width * factor
        self.height = self.height * factor

shape = Rectangle(4, 5)
shape.scale(2)
print(shape.width, shape.height)
```

**Output:**

```
8 10
```

A method with no `return` returns `None`, which is correct for this kind of method and worth knowing so its result is not used by mistake:

```python
result = shape.scale(2)
print(result)
```

**Output:**

```
None
```

This is the same rule as `list.append()` returning nothing while `list.count()` returns a number. Methods that change return nothing; methods that report return a value. Avoid writing methods that do both — they are the ones people misuse.

## Returning Values to Report Success

A changing method may return a value when the caller needs to know what happened:

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def withdraw(self, amount):
        if amount > self.balance:
            return False
        self.balance = self.balance - amount
        return True

account = BankAccount("Anita", 1000)
print(account.withdraw(200))
print(account.withdraw(99999))
print(account.balance)
```

**Output:**

```
True
False
800
```

The caller can act on the result:

```python
if not account.withdraw(99999):
    print("Withdrawal declined")
```

**Output:**

```
Withdrawal declined
```

Returning `True` or `False` suits a condition the caller is expected to handle. Raising an error suits a condition that means a bug — a negative deposit, say. Insufficient funds is a normal outcome of banking; a negative deposit is a mistake in the calling code.

## Methods Calling Methods

A method reaches other methods on the same object through `self`:

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def is_square(self):
        return self.width == self.height

    def describe(self):
        kind = "square" if self.is_square() else "rectangle"
        return f"A {kind} of area {self.area()}"

print(Rectangle(4, 5).describe())
print(Rectangle(4, 4).describe())
```

**Output:**

```
A rectangle of area 20
A square of area 16
```

`describe()` does no arithmetic of its own. It asks the object two questions and assembles the answer, so the rule for area lives in exactly one place.

The `self.` is required. A bare `area()` would look for a function outside the class and raise `NameError`.

## Documenting a Method

A string as the first line of a method body is a **docstring**, describing what the method does:

```python
class BankAccount:
    """A bank account with a running balance."""

    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        """Add amount to the balance."""
        self.balance = self.balance + amount

print(BankAccount.__doc__)
print(BankAccount.deposit.__doc__)
```

**Output:**

```
A bank account with a running balance.
Add amount to the balance.
```

The class gets one too, on the line after `class`. `help(BankAccount)` prints them together, which is how a reader finds out what a class does without reading its code.

Describe what the method does and what it returns, not how it works. The body already says how.

## A Complete Class

Everything so far, in one class:

```python
class BankAccount:
    """A bank account that records its transactions."""

    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance
        self.transactions = []

    def deposit(self, amount):
        """Add amount to the balance. Amount must be positive."""
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self.balance = self.balance + amount
        self.transactions.append(("deposit", amount))

    def withdraw(self, amount):
        """Remove amount if funds allow. Return whether it succeeded."""
        if amount > self.balance:
            return False
        self.balance = self.balance - amount
        self.transactions.append(("withdraw", amount))
        return True

    def summary(self):
        """Return a one-line description of the account."""
        return f"{self.owner}: {self.balance}"

account = BankAccount("Anita", 1000)
account.deposit(500)
account.withdraw(200)
account.withdraw(99999)

print(account.summary())
print(account.transactions)
```

**Output:**

```
Anita: 1300
[('deposit', 500), ('withdraw', 200)]
```

The declined withdrawal left no transaction, because the method returned before recording one.

Everything about an account is now in one place. The data it holds, the rules for changing it, and the way it describes itself are all in the class, and code using an account goes through these four methods.

## Naming

Methods do things, so name them with verbs: `deposit`, `withdraw`, `scale`, `describe`. Methods that answer a yes-or-no question read well starting with `is` or `has`: `is_square`, `has_funds`.

Attributes hold things, so name them with nouns: `balance`, `width`, `transactions`.

A method named like a noun usually wants to be an attribute, and an attribute named like a verb is usually a mistake.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **Instance methods in practice** — https://realpython.com/instance-class-and-static-methods-demystified/

A method is a function in a class, called on an object, with access to that object through `self`. Next, data that belongs to the class rather than to any one object.
