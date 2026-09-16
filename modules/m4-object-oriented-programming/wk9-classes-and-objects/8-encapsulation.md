# Encapsulation

**Encapsulation** is two things: keeping data together with the operations that act on it, and controlling what code outside may do to that data.

The first half is done. A class holds attributes and the methods that use them. The second half is not:

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
account.balance = -99999
print(account.balance)
```

**Output:**

```
-99999
```

The careful check in `withdraw()` was bypassed by not calling it. Every attribute is readable and writable from anywhere, which is where the original dictionary was.

## The Underscore Convention

Python's first answer is a naming convention. A leading underscore marks an attribute as internal — not part of how the class is meant to be used:

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance

    def get_balance(self):
        return self._balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("deposit must be positive")
        self._balance = self._balance + amount

account = BankAccount("Anita", 1000)
account.deposit(500)
print(account.get_balance())
```

**Output:**

```
1500
```

`_balance` is still fully accessible. Nothing stops `account._balance = -99999`. The underscore is a message to the reader: *this is internal, it may change, do not rely on it.*

That sounds weak and works better than it sounds. It is universally understood, tools flag it, and code reviews catch it. The convention is the mechanism.

## Name Mangling

Two leading underscores do something more. Python rewrites the name, inserting the class name:

```python
class Account:
    def __init__(self):
        self.__secret = 42

acc = Account()
print(acc.__dict__)
```

**Output:**

```
{'_Account__secret': 42}
```

The attribute is stored as `_Account__secret`. The name written in the class is not the name on the object.

So the obvious access fails:

```python
print(acc.__secret)
```

**Output:**

```
AttributeError: 'Account' object has no attribute '__secret'
```

Inside the class, `self.__secret` works, because Python applies the same rewriting there.

This is **name mangling**, and it is not privacy. The attribute is reachable by anyone who knows the rule:

```python
print(acc._Account__secret)
```

**Output:**

```
42
```

Name mangling exists to stop a subclass accidentally overwriting an attribute its parent depends on — not to keep secrets. Use one underscore for "internal", and reserve two for the rare case where a name collision in a subclass would be a genuine problem.

Python has no `private` keyword, and adding one has been repeatedly declined. The reasoning is that a determined caller can always get at the data anyway, so the language spends its effort on making intent clear rather than on locks that can be picked. The convention is often summarised as *we are all consenting adults here*.

## Getters and Setters

The classic approach is a pair of methods: one to read, one to write with checking.

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance

    def get_balance(self):
        return self._balance

    def set_balance(self, value):
        if value < 0:
            raise ValueError("balance cannot be negative")
        self._balance = value

account = BankAccount("Anita", 1000)
print(account.get_balance())
account.set_balance(1500)
print(account.get_balance())
account.set_balance(-1)
```

**Output:**

```
1000
1500
ValueError: balance cannot be negative
```

The rule is now enforced. The cost is that every use is a method call, and existing code written as `account.balance` has to be rewritten as `account.get_balance()`.

In languages without a better option this is standard practice. Python has a better option.

## Properties

A **property** is a method that is used as though it were an attribute. `@property` above a method makes it one:

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

reading = Temperature(25)
print(reading.celsius)
```

**Output:**

```
25
```

`celsius` is defined as a method and read without brackets. The call happens behind the dot.

As written, it is **read-only**. There is no way to set it:

```python
reading.celsius = 30
```

**Output:**

```
AttributeError: property 'celsius' of 'Temperature' object has no setter
```

A setter is added with a second method, decorated with the property's name followed by `.setter`:

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("below absolute zero")
        self._celsius = value

reading = Temperature(25)
print(reading.celsius)

reading.celsius = 100
print(reading.celsius)

reading.celsius = -300
```

**Output:**

```
25
100
ValueError: below absolute zero
```

`reading.celsius = 100` looks like plain assignment and runs the setter, so the check cannot be skipped. This is the encapsulation the chapter opened by wanting, with none of the syntactic cost: the interface is `reading.celsius`, exactly as it would be for an ordinary attribute.

Both methods share the property's name, and the real data lives in `_celsius`. Storing it in `self.celsius` instead would make the setter call itself endlessly.

## Computed Properties

A property need not store anything. It can calculate:

```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @property
    def fahrenheit(self):
        return self._celsius * 9 / 5 + 32

reading = Temperature(25)
print(reading.fahrenheit)

reading._celsius = 100
print(reading.fahrenheit)
```

**Output:**

```
77.0
212.0
```

`fahrenheit` is never stored, so it cannot fall out of step with `celsius`. Storing both as ordinary attributes would mean keeping two numbers in agreement forever, and eventually failing to.

This is a good reason to reach for a property even when no validation is needed: a value derived from other values should be computed, not duplicated.

## What to Actually Do

Python's guidance is to start simple and add control only where it earns its place.

**Make attributes public by default.** A plain `self.name = name` is correct for data with no rules. Wrapping every attribute in a property produces noise and protects nothing.

**Use a leading underscore** for attributes that are genuinely internal, so readers know not to depend on them.

**Add a property** when there is a rule to enforce, a value to compute, or an attribute that must be read-only. Because a property looks exactly like an attribute from outside, it can be introduced later without changing any code that uses the class — which is precisely why starting with public attributes is safe.

**Use double underscores rarely**, and only to avoid a name clash in a subclass.

Encapsulation is not about hiding everything. It is about making the class's own methods the sensible way to use it, so that invalid states are hard to reach by accident.

## Further Reading

- **Official reference for `property`** — https://docs.python.org/3/library/functions.html#property
- **Properties and encapsulation in Python** — https://realpython.com/python-property/

Attributes are public unless a class takes steps, underscores signal intent, and a property enforces rules while still reading as an attribute. That completes what a single class can do — next, building one class from another.
