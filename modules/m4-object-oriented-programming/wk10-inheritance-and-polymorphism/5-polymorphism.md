# Polymorphism

**Polymorphism** is one operation working across several types, with each type responding in its own way. The word means "many forms".

The practical benefit is that code can be written against *what* something does without knowing *what it is*.

## It Is Already Everywhere

Built-in operations behave this way:

```python
print(len("hello"))
print(len([1, 2, 3]))
print(len({"a": 1}))
```

**Output:**

```
5
3
1
```

One function, three types, three different pieces of work: counting characters, counting items, counting pairs. `len()` does not test what it was given and choose a branch. Each type knows its own length, and `len()` asks it.

Operators do the same:

```python
print(2 + 3)
print("ab" + "cd")
print([1] + [2])
```

**Output:**

```
5
abcd
[1, 2]
```

Addition, concatenation and joining — one symbol, three behaviours, decided by the operands.

## Polymorphism Through Inheritance

Give several children the same method name, and code that uses it does not need to know which child it has:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound")

class Dog(Animal):
    def speak(self):
        print(f"{self.name} says Woof")

class Cat(Animal):
    def speak(self):
        print(f"{self.name} says Meow")

class Bird(Animal):
    def speak(self):
        print(f"{self.name} says Tweet")

for pet in [Dog("Rex"), Cat("Whiskers"), Bird("Tweety")]:
    pet.speak()
```

**Output:**

```
Rex says Woof
Whiskers says Meow
Tweety says Tweet
```

The loop contains no conditions and mentions no specific class. Each object was asked to speak, and each knew how.

Compare the version without polymorphism:

```python
for pet in pets:
    if isinstance(pet, Dog):
        print(f"{pet.name} says Woof")
    elif isinstance(pet, Cat):
        print(f"{pet.name} says Meow")
    elif isinstance(pet, Bird):
        print(f"{pet.name} says Tweet")
```

This works and has to be edited every time an animal is added — and so does every other place that checks the type. The polymorphic version needs no edit at all: a new class with a `speak` method drops straight into the existing loop.

That is the real argument for polymorphism. **A long chain of `if isinstance(...)` is usually polymorphism waiting to be written.**

## Functions Taking Any Type

A function benefits the same way:

```python
class Dog:
    def speak(self):
        return "Woof"

class Cat:
    def speak(self):
        return "Meow"

def make_it_speak(creature):
    print(creature.speak())

make_it_speak(Dog())
make_it_speak(Cat())
```

**Output:**

```
Woof
Meow
```

`make_it_speak` names no class. It requires only that whatever it is given has a `speak` method.

Note that `Dog` and `Cat` here share no parent. They are unrelated classes that happen to offer the same method, and that was enough.

## Duck Typing

That last point is Python's usual approach, and it has a name: **duck typing**, from the saying *if it walks like a duck and quacks like a duck, it is a duck*.

Python does not check an object's type before calling a method. It looks for the method, and calls it if it is there:

```python
class Duck:
    def speak(self):
        return "Quack"

class Robot:
    def speak(self):
        return "Beep"

class Person:
    def speak(self):
        return "Hello"

for thing in [Duck(), Robot(), Person()]:
    print(thing.speak())
```

**Output:**

```
Quack
Beep
Hello
```

A robot is not an animal and does not inherit from one. It has a `speak` method, which is the only requirement.

In many languages, uniform treatment requires a shared base class or interface. In Python it requires a shared method name. That is more flexible and it is less enforced — nothing declares the requirement, and a missing method is found when it is called:

```python
class Rock:
    pass

print(Rock().speak())
```

**Output:**

```
AttributeError: 'Rock' object has no attribute 'speak'
```

Inheritance and duck typing are both routes to polymorphism, and they are not exclusive. Use a common parent when the classes genuinely share behaviour worth writing once; rely on duck typing when the only thing in common is the method name.

## A Worked Example

Shapes are the standard illustration, because the calculation differs entirely between them while the question does not:

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14159 * self.radius ** 2

class Square:
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side ** 2

class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

shapes = [Circle(2), Square(3), Rectangle(2, 5)]

for shape in shapes:
    print(f"{type(shape).__name__:<10}{shape.area():>8.2f}")

print(f"{'Total':<10}{sum(shape.area() for shape in shapes):>8.2f}")
```

**Output:**

```
Circle       12.57
Square        9.00
Rectangle    10.00
Total        31.57
```

`type(shape).__name__` gets a class's name as a string, which is useful for reporting.

The total is computed without knowing what any shape is. Adding a `Triangle` class with an `area` method requires no change to either the loop or the sum.

## Polymorphism and Overriding

The two are closely related and are not the same thing.

**Overriding** is the mechanism: a child provides its own version of a method.

**Polymorphism** is the result: calling that method on any object in the hierarchy does the right thing without the caller choosing.

Overriding is what you write. Polymorphism is what you get.

## Keeping the Interface Honest

Polymorphism only works while the shared method behaves consistently. Three rules keep it usable:

**Same name.** Obviously — but it is easy to end up with `speak` on one class and `make_sound` on another, and nothing will warn you until the loop fails.

**Same parameters.** If `Dog.speak()` takes no arguments and `Cat.speak(volume)` requires one, code written for animals in general breaks on cats.

**Same kind of result.** If one `area()` returns a number and another prints and returns `None`, `sum(shape.area() for shape in shapes)` fails on the second. Return types should match.

Nothing in Python enforces any of this. The next chapter's subject is the tool that does.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **Polymorphism and duck typing** — https://realpython.com/inheritance-composition-python/

One method name, several types, each with its own implementation — reached through a shared parent or simply through a shared name. Next, a related idea that Python deliberately does not have.
