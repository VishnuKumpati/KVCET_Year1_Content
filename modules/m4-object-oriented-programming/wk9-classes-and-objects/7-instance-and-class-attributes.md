# Instance and Class Attributes

Every attribute so far has belonged to one object. Some data belongs to the class instead — true of every object, and stored once.

An **instance attribute** is set on an object, usually in `__init__` with `self`, and each object has its own.

A **class attribute** is set in the class body, outside any method, and is shared by every object of the class.

## Defining Both

```python
class Dog:
    species = "Canis familiaris"

    def __init__(self, name):
        self.name = name

first = Dog("Rex")
second = Dog("Buddy")

print(first.name, "|", second.name)
print(first.species, "|", second.species)
```

**Output:**

```
Rex | Buddy
Canis familiaris | Canis familiaris
```

`name` differs per dog. `species` does not, and it was written once.

The class attribute belongs to the class, so it can be read without an object at all:

```python
print(Dog.species)
```

**Output:**

```
Canis familiaris
```

`Dog.name` would raise `AttributeError`, because `name` exists only on objects.

The distinction is visible in `__dict__`, which lists only what the object itself holds:

```python
class Dog:
    species = "Canis familiaris"

    def __init__(self, name):
        self.name = name

first = Dog("Rex")
print(first.__dict__)
```

**Output:**

```
{'name': 'Rex'}
```

`species` is not there. Reading `first.species` still worked, which means the lookup went somewhere else.

## How the Lookup Works

Reading an attribute follows a fixed order:

```
first.species
     │
     ├── on the object itself?   →  use it
     └── otherwise, on the class? →  use it
                                  →  otherwise, AttributeError
```

The object is checked first, then its class. That single rule explains everything in this chapter.

## Assigning Through an Object Creates an Instance Attribute

Assignment never follows the lookup chain. It always writes to the thing on the left of the dot:

```python
class Dog:
    species = "Canis familiaris"

    def __init__(self, name):
        self.name = name

first = Dog("Rex")
second = Dog("Buddy")

first.species = "Something else"

print(first.species)
print(second.species)
print(Dog.species)
```

**Output:**

```
Something else
Canis familiaris
Canis familiaris
```

The class attribute was not changed. A new **instance** attribute was created on `first`, and it now hides the class one for that object only:

```python
print(first.__dict__)
```

**Output:**

```
{'name': 'Rex', 'species': 'Something else'}
```

This is called **shadowing**. Removing the instance attribute reveals the class one again:

```python
del first.species
print(first.species)
```

**Output:**

```
Canis familiaris
```

Nothing was restored — the class attribute was never touched. The object simply stopped having its own.

## Assigning Through the Class Changes It for Everyone

To change the shared value, assign to the class:

```python
class Dog:
    species = "Canis familiaris"

    def __init__(self, name):
        self.name = name

first = Dog("Rex")
second = Dog("Buddy")

Dog.species = "Changed"

print(first.species)
print(second.species)
```

**Output:**

```
Changed
Changed
```

Neither object holds `species`, so both find the new class value.

| Written | Effect |
| --- | --- |
| `Dog.species = x` | changes the shared value for every object without its own |
| `first.species = x` | gives `first` its own, hiding the shared one |
| `del first.species` | removes the object's own, revealing the shared one again |

## Counting Objects

A class attribute updated from `__init__` is the standard way to count instances:

```python
class Dog:
    count = 0

    def __init__(self, name):
        self.name = name
        Dog.count = Dog.count + 1

Dog("Rex")
Dog("Buddy")
Dog("Max")

print(Dog.count)
```

**Output:**

```
3
```

Note `Dog.count`, not `self.count`. Writing `self.count = self.count + 1` would read the class value and then create an instance attribute on that one object — every dog would end up with its own `count` of `1`, and `Dog.count` would stay `0`.

This is the general rule: **read through `self`, write through the class.** Reading through `self` works because the lookup falls back to the class; writing through `self` never reaches the class at all.

## The Mutable Class Attribute Trap

Class attributes are shared, and sharing a mutable object is rarely what anyone wants:

```python
class Dog:
    tricks = []

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

first = Dog("Rex")
second = Dog("Buddy")

first.add_trick("sit")

print(first.tricks)
print(second.tricks)
```

**Output:**

```
['sit']
['sit']
```

Buddy learned a trick nobody taught him. There is one list, held by the class, and `self.tricks.append(...)` found it through the lookup chain and changed it.

Note that no assignment happened. `self.tricks.append("sit")` reads `self.tricks` — falling back to the class — and then mutates what it finds. The shadowing rule never came into play, because nothing was assigned.

The fix is to give each object its own list in `__init__`:

```python
class Dog:
    def __init__(self, name):
        self.name = name
        self.tricks = []

    def add_trick(self, trick):
        self.tricks.append(trick)

first = Dog("Rex")
second = Dog("Buddy")

first.add_trick("sit")

print(first.tricks)
print(second.tricks)
```

**Output:**

```
['sit']
[]
```

`self.tricks = []` runs once per object and builds a new list each time.

The rule that avoids this entirely: **class attributes should be immutable.** Strings, numbers, booleans and tuples are safe to share. Lists, dictionaries and sets belong in `__init__`.

## Choosing Between Them

Use a **class attribute** for a value that is genuinely the same for every object and does not vary: a species name, a fixed interest rate, a version string, a set of valid options, a counter of how many objects exist.

Use an **instance attribute** for anything describing one particular object, and for anything mutable.

When in doubt, use an instance attribute. Shared state is the harder thing to reason about, and a value duplicated across objects costs almost nothing.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html#class-and-instance-variables
- **Class and instance attributes compared** — https://realpython.com/python-classes/

Instance attributes belong to one object, class attributes to the class, and the lookup checks the object first. Next, controlling what outside code may do to any of them.
