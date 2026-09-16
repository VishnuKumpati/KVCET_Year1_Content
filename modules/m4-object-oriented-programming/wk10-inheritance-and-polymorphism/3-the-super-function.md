# The super() Function

Overriding a method replaces it. `super()` gives a child access to the version it replaced, so the parent's work can be done as well as the child's rather than instead of it.

## Calling the Parent's __init__

The duplication problem from overriding `__init__` disappears in one line:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

pet = Dog("Rex", "Beagle")
print(pet.__dict__)
pet.speak()
```

**Output:**

```
{'name': 'Rex', 'breed': 'Beagle'}
Rex makes a sound
```

`super().__init__(name)` ran `Animal.__init__` on this object, which set `self.name`. The child then added `self.breed`. Both attributes are on one object, and `self.name = name` appears in exactly one class.

Read the call as: *do what the parent's `__init__` would have done, with these arguments.*

Three details:

**`self` is not passed.** `super().__init__(name)` supplies the current object automatically, exactly as `pet.speak()` does.

**Only the parent's own parameters are passed.** `Animal.__init__` takes `name`, so `name` is what it gets. `breed` means nothing to it.

**It is usually the first line.** Setting up the parent's state before adding to it is the natural order, and it means the child's own assignments can rely on the parent's attributes existing. Python does not require it.

## Extending a Method

The same call works in any method, and this is where `super()` earns its place beyond `__init__`:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def speak(self):
        super().speak()
        print("  (a Woof, specifically)")

Dog("Rex").speak()
```

**Output:**

```
Rex makes a sound
  (a Woof, specifically)
```

The parent's behaviour happened, and the child added to it. The child did not have to know how `Animal.speak` works, only that it should run.

The call can go anywhere in the method — before the child's work, after it, or inside a condition. Putting it after is how a child adds a step that must happen first:

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance

    def describe(self):
        return f"{self.owner}: {self.balance}"

class SavingsAccount(BankAccount):
    def __init__(self, owner, balance=0, rate=0.04):
        super().__init__(owner, balance)
        self.rate = rate

    def describe(self):
        return super().describe() + f" (savings at {self.rate:.0%})"

account = SavingsAccount("Anita", 1000)
print(account.describe())
```

**Output:**

```
Anita: 1000 (savings at 4%)
```

`SavingsAccount.describe` builds on the parent's string rather than reproducing it. Changing how a `BankAccount` describes itself changes this too, automatically.

## Why Not Name the Parent Directly

The parent can be called by name, and it works:

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        Animal.__init__(self, name)
        self.breed = breed

print(Dog("Rex", "Beagle").__dict__)
```

**Output:**

```
{'name': 'Rex', 'breed': 'Beagle'}
```

Note that `self` must be passed explicitly here. `Animal.__init__` is being reached through the class rather than through an object, so nothing supplies it.

Three reasons to prefer `super()` anyway.

**The parent's name is written once.** With `Animal.__init__(self, ...)`, changing what `Dog` inherits from means finding and editing every such call. `super()` needs no edit at all.

**It works correctly with multiple parents.** When a class has more than one parent, `super()` follows the full search order, so every class in the hierarchy is reached exactly once. Hard-coded parent calls visit some classes twice and others not at all.

**It is what readers expect.** `super()` is the standard form; a named parent call reads as though something unusual is going on.

Use `super()`. The named form appears in older code and in the rare case where a specific parent must be chosen deliberately.

## super() Follows the Search Order

`super()` does not simply mean "my parent". It means "the next class in this object's search order after the current one".

With a single chain of parents those are the same thing. With several parents they are not, and the difference is what makes `super()` correct in hierarchies where a hand-written call would go wrong. The search order and what it means are the next chapter.

## Forgetting to Call It

Nothing requires `super().__init__()`. Leaving it out is legal, silent, and the cause of the `AttributeError` seen earlier:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def __init__(self, name, breed):
        self.breed = breed

Dog("Rex", "Beagle").speak()
```

**Output:**

```
AttributeError: 'Dog' object has no attribute 'name'
```

The object was created without complaint. The failure came later, in a method that was inherited correctly and depends on state that was never set.

The habit worth forming: **when a child defines `__init__`, the first line calls `super().__init__(...)`** unless there is a deliberate reason not to.

A child that does not need its own `__init__` should not define one. Inheriting the parent's is the simplest correct option:

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Cat(Animal):
    def meow(self):
        print(f"{self.name} says Meow")

Cat("Whiskers").meow()
```

**Output:**

```
Whiskers says Meow
```

## Further Reading

- **Official reference for `super()`** — https://docs.python.org/3/library/functions.html#super
- **Using super() effectively** — https://realpython.com/python-super/

`super()` runs the parent's version of a method, letting a child extend rather than replace, and keeps the parent's name out of the child's code. Next, the shapes a hierarchy can take.
