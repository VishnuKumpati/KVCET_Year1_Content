# Inheritance

Two classes often turn out to be mostly the same class:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

    def sleep(self):
        print(f"{self.name} is sleeping")

    def bark(self):
        print(f"{self.name} says Woof")

class Cat:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

    def sleep(self):
        print(f"{self.name} is sleeping")

    def meow(self):
        print(f"{self.name} says Meow")
```

Three of the four methods are identical. Adding a `drink()` method means writing it twice, and fixing a bug in `eat()` means remembering to fix it in both places. A third animal makes it three.

**Inheritance** removes the duplication. One class is defined with the shared parts, and others are built from it, keeping what it has and adding their own.

## The Syntax

The parent class is named in brackets after the child's name:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

    def sleep(self):
        print(f"{self.name} is sleeping")

class Dog(Animal):
    def bark(self):
        print(f"{self.name} says Woof")

class Cat(Animal):
    def meow(self):
        print(f"{self.name} says Meow")
```

`Dog` defines one method and has four:

```python
pet = Dog("Rex")
pet.eat()
pet.sleep()
pet.bark()
```

**Output:**

```
Rex is eating
Rex is sleeping
Rex says Woof
```

`eat` and `sleep` were never written in `Dog`. Neither was `__init__`, which is why `Dog("Rex")` works and why `self.name` exists for `bark` to use.

The shared behaviour is written once. A bug fixed in `Animal.eat()` is fixed for every animal, and a new `drink()` method added to `Animal` appears on all of them at once.

## The Terminology

Three pairs of words mean the same thing, and all three are in common use:

| The class being built from | The class being built |
| --- | --- |
| parent | child |
| base class | derived class |
| superclass | subclass |

`Animal` is the parent, base class and superclass. `Dog` is the child, derived class and subclass. This chapter uses parent and child.

## What Is Inherited

A child gets the parent's methods and class attributes:

```python
class Animal:
    kingdom = "Animalia"

    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

class Dog(Animal):
    pass

pet = Dog("Rex")
pet.eat()
print(pet.kingdom)
print(Dog.kingdom)
```

**Output:**

```
Rex is eating
Animalia
Animalia
```

`Dog` has no body of its own beyond `pass`, and it is a complete working class.

Instance attributes are not inherited, because they do not belong to a class at all — they are created on each object when `__init__` runs. What a child inherits is the `__init__` that creates them.

Inheritance is one-way. The parent knows nothing about its children:

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def bark(self):
        print("Woof")

generic = Animal("Thing")
generic.bark()
```

**Output:**

```
AttributeError: 'Animal' object has no attribute 'bark'
```

So does a sibling. A `Cat` does not get `Dog`'s methods:

```python
pet = Dog("Rex")
pet.meow()
```

**Output:**

```
AttributeError: 'Dog' object has no attribute 'meow'
```

## How the Lookup Works

Reading an attribute or calling a method searches the object, then its class, then that class's parent, and on upwards:

```
pet.eat()
  │
  ├── on the object itself?     no
  ├── on Dog?                   no
  ├── on Animal?                yes  →  use it
  └── otherwise, AttributeError
```

This is the same rule as before with one extra step: after the class, the search continues to the parent. That single extension is all inheritance is at the mechanical level.

The full search order is stored on the class as `__mro__`:

```python
class Animal:
    pass

class Dog(Animal):
    pass

print(Dog.__mro__)
```

**Output:**

```
(<class '__main__.Dog'>, <class '__main__.Animal'>, <class 'object'>)
```

Dog, then Animal, then `object`. The direct parents alone are in `__bases__`:

```python
print(Dog.__bases__)
```

**Output:**

```
(<class '__main__.Animal'>,)
```

## object Is the Root

`object` appears at the end of every search order because every class inherits from it, whether or not anything is written:

```python
class Animal:
    pass

print(Animal.__bases__)
print(issubclass(Animal, object))
print(isinstance(5, object))
```

**Output:**

```
(<class 'object'>,)
True
True
```

`object` is where the default behaviour of every class comes from — the reason a new object can be created at all, the reason `==` compares identity by default, and the reason printing one shows a class name and an address. Overriding those defaults is what a later chapter is about.

## Testing the Relationship

`isinstance()` is true for an object's own class **and** every class above it:

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    pass

class Cat(Animal):
    pass

pet = Dog("Rex")
print(isinstance(pet, Dog))
print(isinstance(pet, Animal))
print(isinstance(pet, Cat))
```

**Output:**

```
True
True
False
```

A Dog is an Animal. That is the point of the relationship, and it is why `isinstance()` is preferred over `type(pet) == Animal`, which would report `False` and be unhelpful about it.

`issubclass()` asks the same question about two classes:

```python
print(issubclass(Dog, Animal))
print(issubclass(Animal, Dog))
print(issubclass(Dog, object))
```

**Output:**

```
True
False
True
```

## When Inheritance Is the Wrong Tool

Inheritance says **is-a**. A Dog is an Animal, a Manager is an Employee, a SavingsAccount is a BankAccount. If that sentence is not true, inheritance is the wrong relationship.

The alternative is **has-a**, where one object holds another as an attribute. This is called **composition**:

```python
class Engine:
    def start(self):
        print("Engine started")

class Car:
    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car ready")

Car().start()
```

**Output:**

```
Engine started
Car ready
```

A Car is not a kind of Engine, so `class Car(Engine)` would be wrong even though it would run. A Car *has* an Engine.

Composition is the more flexible of the two and the one to reach for when neither obviously fits. Inheritance ties a child to its parent permanently: every change to the parent reaches every child, which is exactly what you want when the relationship is genuine and a liability when it is not.

Two warning signs that inheritance is being misused: a child that overrides most of what it inherits, and a hierarchy more than two or three levels deep.

## Further Reading

- **Official Python guide to inheritance** — https://docs.python.org/3/tutorial/classes.html#inheritance
- **Inheritance and composition compared** — https://realpython.com/inheritance-composition-python/

A child class keeps everything its parent has and adds its own, with the attribute lookup continuing up the chain. Next, changing something a child inherited.
