# The __init__ Method

Setting attributes from outside leaves a class saying nothing about what its objects hold. `__init__` fixes that: it is a method Python runs automatically whenever an object is created, and it is where an object's attributes are established.

## The Syntax

`__init__` is defined like any method, with `self` first and then whatever the object needs:

```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
```

The name is `init` with two underscores either side. Methods named this way are called **dunder** methods, short for double underscore. The double underscores mark a name that Python itself calls — you write `__init__`, and Python decides when it runs.

Arguments given when the class is called are passed to it:

```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age

pet = Dog("Rex", "Beagle", 3)
print(pet.name)
print(pet.__dict__)
```

**Output:**

```
Rex
{'name': 'Rex', 'breed': 'Beagle', 'age': 3}
```

`Dog("Rex", "Beagle", 3)` built an object and ran `__init__` on it, with `name` as `"Rex"`, `breed` as `"Beagle"` and `age` as `3`. Nothing called `__init__` explicitly. It was never mentioned outside the class.

Three lines of setup per object have become one line, and every Dog is now guaranteed to have all three attributes.

## Parameters Against Attributes

The repetition in `self.name = name` confuses almost everyone once. The two sides are different things:

```python
self.name = name
#    ↑         ↑
#    │         └── the parameter: a local variable, gone when __init__ ends
#    └── the attribute: stored on the object, lasts as long as the object
```

The parameter is ordinary and temporary. The assignment copies its value onto the object, where it survives.

They do not have to share a name, and giving them the same name is the convention because it reads well:

```python
class Dog:
    def __init__(self, dog_name):
        self.name = dog_name

pet = Dog("Rex")
print(pet.name)
```

**Output:**

```
Rex
```

What matters is the `self.` prefix. Without it, nothing is stored:

```python
class Dog:
    def __init__(self, name):
        name = name

pet = Dog("Rex")
print(pet.name)
```

**Output:**

```
AttributeError: 'Dog' object has no attribute 'name'
```

`name = name` assigned a local variable to itself and then discarded it when `__init__` finished. The object was never given anything. Forgetting `self.` is the commonest mistake in a first class, and this `AttributeError` is what it looks like.

## Arguments Are Now Required

The definition states what a Dog needs, and Python enforces it:

```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age

pet = Dog("Rex")
```

**Output:**

```
TypeError: Dog.__init__() missing 2 required positional arguments: 'breed' and 'age'
```

A half-built object is now impossible. The error arrives at the point of the mistake, naming exactly what was left out, rather than surfacing later as a missing attribute.

Too many arguments is an error as well:

```python
pet = Dog("Rex", "Beagle", 3, "extra")
```

**Output:**

```
TypeError: Dog.__init__() takes 4 positional arguments but 5 were given
```

Four and five, for a call that passed three and four. `self` is counted, because Python supplies it — which is the subject of the next chapter.

## Default Values

Parameters can have defaults, making those arguments optional:

```python
class Dog:
    def __init__(self, name, breed="Unknown", age=0):
        self.name = name
        self.breed = breed
        self.age = age

first = Dog("Rex", "Beagle", 3)
second = Dog("Buddy")

print(first.__dict__)
print(second.__dict__)
```

**Output:**

```
{'name': 'Rex', 'breed': 'Beagle', 'age': 3}
{'name': 'Buddy', 'breed': 'Unknown', 'age': 0}
```

Both objects have all three attributes. The second took the defaults for two of them.

Parameters with defaults must come after those without, and keyword arguments let one be supplied without the others:

```python
class Dog:
    def __init__(self, name, breed="Unknown", age=0):
        self.name = name
        self.breed = breed
        self.age = age

pet = Dog("Buddy", age=5)
print(pet.__dict__)
```

**Output:**

```
{'name': 'Buddy', 'breed': 'Unknown', 'age': 5}
```

Use a default when a sensible one exists. Leave a parameter required when there is no reasonable value — a bank account with a default owner would be a bug waiting to happen.

## Attributes Not Taken from Parameters

Not every attribute comes from an argument. Anything an object should start with can be set outright:

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance
        self.transactions = []
        self.is_active = True

account = BankAccount("Anita", 1000)
print(account.__dict__)
```

**Output:**

```
{'owner': 'Anita', 'balance': 1000, 'transactions': [], 'is_active': True}
```

`transactions` and `is_active` are part of what an account is, and neither is something a caller should have to supply. Note that `self.transactions = []` builds a **new** empty list every time `__init__` runs, so each account gets its own.

## Checking the Arguments

`__init__` is an ordinary method body, so it can contain any code — including checks. `raise` reports an error and stops:

```python
class BankAccount:
    def __init__(self, owner, balance):
        if balance < 0:
            raise ValueError("balance cannot be negative")
        self.owner = owner
        self.balance = balance

account = BankAccount("Anita", -500)
```

**Output:**

```
ValueError: balance cannot be negative
```

`raise ValueError("...")` produces the same kind of error Python raises itself, with your message attached.

Checking here is worth doing because `__init__` is the only way in. An object that gets past it is valid, so nothing afterwards has to re-check. An invalid object is never created at all — compare that with the dictionary version, where an impossible balance could be written at any moment.

## Two Details

**`__init__` does not create the object.** Python creates it first, then calls `__init__` on the already-existing object to set it up. That is why the method is called *initialise* rather than *construct*, and why `self` is available inside it. The distinction rarely matters; the name makes more sense once you know it.

**`__init__` returns nothing.** Its job is to set attributes on `self`, and returning a value is an error:

```python
class Dog:
    def __init__(self, name):
        self.name = name
        return name
```

**Output:**

```
TypeError: __init__() should return None, not 'str'
```

`Dog("Rex")` gives back the new object. That is arranged by Python, not by anything `__init__` returns.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **The `__init__` method explained** — https://realpython.com/python-class-constructor/

`__init__` runs automatically at creation, takes the arguments given to the class, and stores them on the object with `self`. Next, what `self` actually is.
