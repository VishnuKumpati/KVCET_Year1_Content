# Attributes

An object that holds no data is not much use. An **attribute** is a variable belonging to an object — the data that makes one object different from another of the same class.

Attributes are reached with a dot, the same notation that calls a method. The difference is that a method is called and an attribute is not:

```
object.attribute     the data
object.method()      the operation
```

## Setting and Reading

Assignment with a dot creates an attribute on an object:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
pet.breed = "Beagle"

print(pet.name)
print(pet.breed)
```

**Output:**

```
Rex
Beagle
```

The class defined nothing. Both attributes were added to this one object after it was built.

Reading uses the same notation without the assignment, and an attribute can be used anywhere a value can:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
pet.age = 3

print(f"{pet.name} is {pet.age} years old")
print(pet.age * 7)
```

**Output:**

```
Rex is 3 years old
21
```

Assigning to an attribute that already exists replaces its value:

```python
class Dog:
    pass

pet = Dog()
pet.age = 3
pet.age = 4
print(pet.age)
```

**Output:**

```
4
```

So assignment creates or updates, exactly as it does for a dictionary key.

## AttributeError

Reading an attribute that does not exist is an error:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
print(pet.breed)
```

**Output:**

```
AttributeError: 'Dog' object has no attribute 'breed'
```

This is the improvement over a dictionary that the previous chapter promised. A misspelling is caught:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
print(pet.nmae)
```

**Output:**

```
AttributeError: 'Dog' object has no attribute 'nmae'
```

The error names the attribute and the class, which is usually enough to find the problem immediately.

Note that the protection is one-sided. *Reading* a misspelled attribute raises; *writing* one still creates it silently:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
pet.nmae = "Buddy"
print(pet.name)
```

**Output:**

```
Rex
```

`pet.nmae` was created, `pet.name` was not changed, and nothing complained. Defining attributes in one place rather than scattering assignments through a program is what avoids this, and that is what the next chapter is for.

## Each Object Has Its Own

Attributes belong to the object, not the class. Two objects of the same class carry separate data:

```python
class Dog:
    pass

first = Dog()
first.name = "Rex"

second = Dog()
second.name = "Buddy"

print(first.name)
print(second.name)
```

**Output:**

```
Rex
Buddy
```

Changing one leaves the other alone:

```python
first.name = "Max"
print(first.name)
print(second.name)
```

**Output:**

```
Max
Buddy
```

This is the point of objects. One class, many objects, each holding its own state.

The usual warning about names applies. Two names for one object are two names for one set of attributes:

```python
class Dog:
    pass

first = Dog()
first.name = "Rex"
alias = first
alias.name = "Max"

print(first.name)
```

**Output:**

```
Max
```

`alias = first` copied no object, so there is one Dog with two names pointing at it.

## Checking Without Raising

`hasattr()` tests whether an object has an attribute, returning `True` or `False`:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"

print(hasattr(pet, "name"))
print(hasattr(pet, "breed"))
```

**Output:**

```
True
False
```

`getattr()` reads an attribute and takes a default for when it is missing, which is what `get()` does for a dictionary:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"

print(getattr(pet, "name"))
print(getattr(pet, "breed", "unknown"))
```

**Output:**

```
Rex
unknown
```

Both take the attribute name as a **string**, which is what allows the name to be computed rather than written out. Use them when an attribute is genuinely optional. When it is not optional, `pet.breed` and the `AttributeError` it raises are clearer — an error at the point of the mistake beats a default that hides it.

## Seeing Every Attribute

An object keeps its attributes in a dictionary, available as `__dict__`:

```python
class Dog:
    pass

pet = Dog()
pet.name = "Rex"
pet.age = 3

print(pet.__dict__)
```

**Output:**

```
{'name': 'Rex', 'age': 3}
```

That is a useful debugging tool: it shows everything an object currently holds, in one line, without guessing attribute names.

It also explains the behaviour above. `pet.name` is a lookup in that dictionary, which is why a missing name raises and a new name can be created. The dot notation is the interface; a dictionary is what sits behind it.

## Why This Is Not Enough

Setting attributes from outside works and does not scale. Every object has to be built by hand:

```python
class Dog:
    pass

first = Dog()
first.name = "Rex"
first.breed = "Beagle"
first.age = 3

second = Dog()
second.name = "Buddy"
second.breed = "Boxer"
second.age = 5
```

Three lines of setup per object, repeated everywhere a Dog is created. Three problems follow.

**Nothing requires the attributes.** A Dog with no `name` is perfectly legal, and the gap is found later, by whatever tried to read it.

**Nothing keeps them consistent.** One part of a program might set `age`, another `years`. Both work, and nothing notices.

**The class says nothing.** Reading `class Dog: pass` tells you nothing about what a Dog has. The real definition is scattered across every place one is built.

What is needed is a way to say, in the class itself, what every object of that class must be given when it is created — and to have that setup run automatically.

## Further Reading

- **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html
- **Attributes and instance state** — https://realpython.com/python-classes/

An attribute is a variable belonging to an object, reached with a dot, and each object has its own. Next, the method that gives every new object its attributes automatically.
