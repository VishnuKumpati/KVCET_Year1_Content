# self

`self` has appeared in every method so far without being explained. It is the single most confusing part of a first class, and the explanation is short: **`self` is the object the method was called on.**

## One Definition, Many Objects

A method is written once, in the class. It has to work for every object built from that class, so it needs a way to refer to whichever object it is currently dealing with. That is `self`:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} says Woof")

first = Dog("Rex")
second = Dog("Buddy")

first.bark()
second.bark()
```

**Output:**

```
Rex says Woof
Buddy says Woof
```

One `bark` method, two different outputs. On the first call `self` was the Rex object; on the second it was the Buddy object. The method reads `self.name`, and `self` decides whose name that is.

## Python Passes It For You

`bark` is defined with one parameter and called with none. Nothing is missing — Python supplies the object automatically.

These two lines do the same thing:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} says Woof")

pet = Dog("Rex")

pet.bark()
Dog.bark(pet)
```

**Output:**

```
Rex says Woof
Rex says Woof
```

The second form is what the first becomes. `pet.bark()` means *call `bark` from `Dog`, with `pet` as `self`*. The object to the left of the dot is passed as the first argument.

Nobody writes the second form in practice. It is shown because it makes the rule concrete: the dot is what supplies `self`.

Proving it is the same object takes one method:

```python
class Dog:
    def whoami(self):
        return self

pet = Dog()
print(pet.whoami() is pet)
```

**Output:**

```
True
```

`self` inside the method and `pet` outside it are two names for one object.

## Why the Argument Counts Look Wrong

This is why an earlier error message counted one more argument than the call appeared to pass:

```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age

pet = Dog("Rex", "Beagle", 3, "extra")
```

**Output:**

```
TypeError: Dog.__init__() takes 4 positional arguments but 5 were given
```

Four parameters: `self`, `name`, `breed`, `age`. Five arguments: the object Python supplied, plus the four written. When an argument-count error is off by one, `self` is the explanation.

## Forgetting self

A method defined without `self` cannot receive the object, and calling it fails:

```python
class Dog:
    def bark():
        print("Woof")

pet = Dog()
pet.bark()
```

**Output:**

```
TypeError: Dog.bark() takes 0 positional arguments but 1 was given
```

Nothing was passed at the call, and Python still supplied the object, so the method received one argument it had no parameter for.

Read this message as: *I gave the method the object and it had nowhere to put it.* The fix is always to add `self` as the first parameter.

Every method needs it, including ones that never use it:

```python
class Dog:
    def bark(self):
        print("Woof")

Dog().bark()
```

**Output:**

```
Woof
```

`self` is unused here and still required, because the call passes it regardless.

## self Is Not a Keyword

`self` is an ordinary parameter name. Python passes the object to whatever the first parameter is called:

```python
class Dog:
    def __init__(this, name):
        this.name = name

    def bark(this):
        print(f"{this.name} says Woof")

Dog("Rex").bark()
```

**Output:**

```
Rex says Woof
```

This works and you should never write it. `self` is universal in Python, every reader expects it, and tools assume it. The fact that it is a convention explains why it must be written out every time — Python cannot supply a name it does not know.

## self.name Against name

Inside a method, `self.name` and `name` are unrelated. One is the object's attribute; the other is a local variable:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def rename(self, name):
        name = name

    def rename_properly(self, name):
        self.name = name

pet = Dog("Rex")

pet.rename("Max")
print(pet.name)

pet.rename_properly("Max")
print(pet.name)
```

**Output:**

```
Rex
Max
```

`rename` assigned a local variable and changed nothing about the object. `rename_properly` assigned the attribute. The difference is three characters and it is the difference between working and not.

The rule: **to touch the object, go through `self`.** Anything else is a local variable that disappears when the method ends.

## Methods Calling Methods

A method reaches other methods on the same object through `self`, exactly as it reaches attributes:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def bark(self):
        print(f"{self.name} says Woof")

    def greet(self):
        print("A dog approaches.")
        self.bark()

Dog("Rex").greet()
```

**Output:**

```
A dog approaches.
Rex says Woof
```

`self.bark()` is required. A bare `bark()` would look for an ordinary function of that name outside the class and raise `NameError`. Methods are reached through the object, including from inside the class that defines them.

## Summary

| Written | Means |
| --- | --- |
| `self` | the object this method was called on |
| `self.name` | that object's `name` attribute |
| `name` | a local variable inside the method |
| `self.bark()` | call this object's `bark` method |
| `pet.bark()` | `Dog.bark(pet)` — the dot supplies `self` |

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html#method-objects
- **What self really is** — https://realpython.com/python-classes/

`self` is the object the method was called on, passed automatically by the dot, and it is the only route to that object's data. Next, the methods themselves.
