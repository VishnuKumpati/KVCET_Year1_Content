# Method Overriding

A child inherits its parent's methods. Sometimes one of them is wrong for the child.

**Overriding** is defining a method in a child with the same name as one in its parent. The child's version is the one that runs.

## Defining an Override

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def speak(self):
        print(f"{self.name} says Woof")

class Cow(Animal):
    pass

Animal("Generic").speak()
Dog("Rex").speak()
Cow("Daisy").speak()
```

**Output:**

```
Generic makes a sound
Rex says Woof
Daisy makes a sound
```

`Dog` overrode `speak`, so its version ran. `Cow` did not, so it inherited the parent's.

Nothing marks an override as one. Defining a method with a name the parent already uses is all it takes.

## Why It Works

The attribute lookup explains it. The search starts at the object, then its class, then upwards, and stops at the first match:

```
Dog("Rex").speak()
  │
  ├── on the object?   no
  ├── on Dog?          yes  →  use it, stop searching
  └── Animal is never reached
```

The parent's `speak` still exists and is still reachable. It is simply never found, because the search ends before it.

`Cow` has no `speak` of its own, so the search continues one level up and finds `Animal`'s.

## The Signature Can Differ

Python does not require an override to take the same parameters:

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self, times=1):
        for count in range(times):
            print("Woof")

Dog().speak()
Dog().speak(3)
```

**Output:**

```
Woof
Woof
Woof
Woof
```

This is allowed and is usually a bad idea. Code written to work with any Animal will call `speak()` with no arguments, so an override that *requires* an extra argument breaks the moment a Dog is used where an Animal was expected. Keep the parameters compatible: add optional ones if you must, never new required ones.

## Overriding __init__

`__init__` is a method, so it overrides like any other. This is the common case, because a child usually needs its own attributes:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def __init__(self, breed):
        self.breed = breed

pet = Dog("Beagle")
print(pet.breed)
pet.speak()
```

**Output:**

```
Beagle
AttributeError: 'Dog' object has no attribute 'name'
```

The `breed` was set and the `name` was not. `Dog.__init__` replaced `Animal.__init__` entirely, so the line that sets `self.name` never ran. `speak()` was inherited successfully and then failed, because the attribute it depends on does not exist.

This is the trap in overriding `__init__`: replacing it discards the parent's setup, and the failure surfaces later, in an inherited method that looks correct.

The obvious repair is to do the parent's work again by hand:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

pet = Dog("Rex", "Beagle")
pet.speak()
```

**Output:**

```
Rex makes a sound
```

That works and reintroduces the duplication inheritance was meant to remove. `self.name = name` now appears in two classes, and a change to how `Animal` initialises itself — a validation check, an extra attribute — has to be copied into every child.

The next chapter solves this properly.

## Replacing Against Extending

An override can discard the parent's behaviour or build on it.

`Dog.speak` above **replaces**: the parent's version plays no part.

Often the child wants the parent's behaviour *and* something extra — logging a transaction, adding a field, checking a further condition. Written with no help from the language, that means copying the parent's body into the child, which is the same duplication problem again.

Both cases need a way to say "do what the parent would have done, then continue". That is `super()`.

## Overriding Is Not Overloading

Two similar words, two unrelated ideas.

**Overriding** is a child redefining a method its parent has. Two classes, one method name, and the child's wins.

**Overloading** is several methods with the same name in one class, distinguished by their parameters. Python does not have it — a later definition simply replaces the earlier one.

Overriding is the one covered here, and it is the one that matters for inheritance.

## Further Reading

- **Official Python guide to inheritance** — https://docs.python.org/3/tutorial/classes.html#inheritance
- **Method overriding explained** — https://www.programiz.com/python-programming/methods/built-in/super

A child's method of the same name is found first, so it runs instead of the parent's — and overriding `__init__` without care loses everything the parent set up. Next, calling the parent's version on purpose.
