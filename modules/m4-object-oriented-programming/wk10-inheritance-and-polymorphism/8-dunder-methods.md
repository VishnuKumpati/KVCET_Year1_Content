# Dunder Methods

A **dunder method** has two underscores either side of its name — `__init__`, `__str__`, `__len__`. The word is short for double underscore, and the defining feature is that **you do not call them; Python does**.

`__init__` is one already. It is never written as `account.__init__(...)`; it runs because an object was created.

Every dunder method works this way. Each attaches your class to a piece of Python syntax: `len(x)` calls `x.__len__()`, `a + b` calls `a.__add__(b)`, `print(x)` calls `x.__str__()`. Defining them is how a class becomes usable with the language's own operations instead of only with methods you invented.

This is also what makes `len()` polymorphic. It does not know about lists — it asks whatever it was given for its `__len__`.

## Printing: __str__ and __repr__

By default, printing an object is unhelpful:

```python
class Plain:
    pass

print(Plain())
```

**Output:**

```
<__main__.Plain object at 0x000001F3D7D61E80>
```

A class name and a memory address, which differs every run. That default comes from `object`.

`__str__` defines what `print()` and `str()` show:

```python
class Money:
    def __init__(self, amount, currency="INR"):
        self.amount = amount
        self.currency = currency

    def __str__(self):
        return f"{self.amount:.2f} {self.currency}"

print(Money(100))
```

**Output:**

```
100.00 INR
```

`__str__` must **return** a string. Printing inside it and returning nothing is a common early mistake and raises a `TypeError`.

`__repr__` is the other one, and it serves a different audience. `__str__` is for users; `__repr__` is for programmers, and should ideally look like the code that would recreate the object:

```python
class Money:
    def __init__(self, amount, currency="INR"):
        self.amount = amount
        self.currency = currency

    def __str__(self):
        return f"{self.amount:.2f} {self.currency}"

    def __repr__(self):
        return f"Money({self.amount}, '{self.currency}')"

item = Money(100)
print(item)
print(repr(item))
print([Money(100), Money(50)])
```

**Output:**

```
100.00 INR
Money(100, 'INR')
[Money(100, 'INR'), Money(50, 'INR')]
```

The third line is the reason `__repr__` matters. Printing a **container** shows the `__repr__` of its contents, never the `__str__`. A list of objects with no `__repr__` prints as a list of memory addresses, which is exactly when you most want to see what is inside.

`__repr__` is also what an interactive prompt shows, and what a debugger displays.

If only one is defined, define `__repr__`. Python falls back to it when `__str__` is missing, but not the other way round.

## Comparison: __eq__ and __lt__

By default, `==` on your own class compares identity — two objects are equal only if they are the same object. `__eq__` changes that to compare contents:

```python
class Money:
    def __init__(self, amount, currency="INR"):
        self.amount = amount
        self.currency = currency

    def __eq__(self, other):
        return self.amount == other.amount and self.currency == other.currency

print(Money(100) == Money(100))
print(Money(100) == Money(50))
```

**Output:**

```
True
False
```

Two separate objects now compare equal because their contents match. `!=` follows automatically — Python derives it from `__eq__`.

One consequence is worth knowing: defining `__eq__` makes the class unhashable, so its objects cannot go in a set or be used as dictionary keys:

```python
print({Money(100)})
```

**Output:**

```
TypeError: unhashable type: 'Money'
```

Objects that compare equal must hash equally, and Python cannot work out how to hash yours, so it withdraws the default rather than behave inconsistently. Defining `__hash__` alongside `__eq__` restores it.

`__lt__` defines `<`, and that is enough to make a class sortable:

```python
class Money:
    def __init__(self, amount, currency="INR"):
        self.amount = amount
        self.currency = currency

    def __repr__(self):
        return f"Money({self.amount}, '{self.currency}')"

    def __lt__(self, other):
        return self.amount < other.amount

print(sorted([Money(100), Money(50), Money(75)]))
```

**Output:**

```
[Money(50, 'INR'), Money(75, 'INR'), Money(100, 'INR')]
```

`sorted()`, `min()` and `max()` all work from `<` alone, so one method unlocks all three.

## Arithmetic: __add__ and Friends

Defining `__add__` gives `+` a meaning for your class. This is **operator overloading**:

```python
class Money:
    def __init__(self, amount, currency="INR"):
        self.amount = amount
        self.currency = currency

    def __str__(self):
        return f"{self.amount:.2f} {self.currency}"

    def __add__(self, other):
        return Money(self.amount + other.amount, self.currency)

print(Money(100) + Money(50))
```

**Output:**

```
150.00 INR
```

`a + b` calls `a.__add__(b)`. Note that a **new** object is returned rather than `self` being modified — that is how `+` behaves for numbers, strings and lists, and a class that broke the pattern would surprise everyone.

The same applies to `-`, `*`, `/` and the rest:

| Operator | Method |
| --- | --- |
| `+` | `__add__` |
| `-` | `__sub__` |
| `*` | `__mul__` |
| `/` | `__truediv__` |
| `//` | `__floordiv__` |
| `%` | `__mod__` |
| `**` | `__pow__` |

Only define an operator where the meaning is genuinely obvious. Adding two amounts of money is clear. Adding two customers is not, and a method named `merge` would say far more than `+`.

## Container Behaviour

Four dunders make a class behave like a collection:

```python
class Playlist:
    def __init__(self, name):
        self.name = name
        self.songs = []

    def add(self, song):
        self.songs.append(song)

    def __len__(self):
        return len(self.songs)

    def __getitem__(self, index):
        return self.songs[index]

    def __contains__(self, song):
        return song in self.songs

    def __str__(self):
        return f"{self.name} ({len(self.songs)} songs)"

playlist = Playlist("Focus")
playlist.add("One")
playlist.add("Two")
playlist.add("Three")

print(len(playlist))
print(playlist[0])
print(playlist[-1])
print("Two" in playlist)
print(playlist)
```

**Output:**

```
3
One
Three
True
Focus (3 songs)
```

`len()`, indexing, negative indexing and `in` all work on a class that is not a list, because each was given a definition.

`__getitem__` does more than indexing. It also makes the object **iterable**:

```python
for song in playlist:
    print(song)
```

**Output:**

```
One
Two
Three
```

No `__iter__` was written. Python falls back to calling `__getitem__` with `0`, `1`, `2` and so on until an `IndexError` stops it — which the underlying list raises for free.

Slicing comes along too, since the index is passed straight through:

```python
print(playlist[0:2])
```

**Output:**

```
['One', 'Two']
```

## Truthiness

`__bool__` decides how an object behaves in a condition. Without it, Python falls back to `__len__`, treating empty as false:

```python
full = Playlist("Focus")
full.add("One")
empty = Playlist("Empty")

print(bool(full))
print(bool(empty))

if empty:
    print("has songs")
else:
    print("no songs")
```

**Output:**

```
True
False
no songs
```

This is why an empty list, string or dictionary is falsy — the same rule, applied to the built-in types.

Define `__bool__` only when emptiness is not the right test. An account might be falsy when closed rather than when it holds no transactions.

## The Common Dunder Methods

| Method | Called by |
| --- | --- |
| `__init__` | creating an object |
| `__str__` | `print(x)`, `str(x)`, f-strings |
| `__repr__` | `repr(x)`, the interactive prompt, containers |
| `__len__` | `len(x)` |
| `__bool__` | `if x:`, `bool(x)` |
| `__eq__` | `x == y` |
| `__lt__` | `x < y`, `sorted()`, `min()`, `max()` |
| `__add__` | `x + y` |
| `__getitem__` | `x[i]`, `x[a:b]`, iteration |
| `__contains__` | `y in x` |
| `__call__` | `x()` |
| `__enter__`, `__exit__` | `with x:` |

There are many more. These are the ones worth knowing by name.

## Restraint

Dunder methods make a class feel like part of the language, and that is the whole point — `playlist[0]` reads better than `playlist.get_song_at(0)`, and `total + payment` better than `total.add_to(payment)`.

The same power makes a class incomprehensible when the meanings are invented. Two rules keep it useful:

**Only define what the syntax already means.** `+` combines, `<` orders, `len()` counts, `in` tests membership. A `__add__` that sends an email is legal and indefensible.

**Start with `__repr__`.** It costs one method, it makes every debugging session and every printed list readable, and it is almost always the first dunder a class should gain after `__init__`.

## Further Reading

- **Official reference for special methods** — https://docs.python.org/3/reference/datamodel.html#special-method-names
- **Dunder methods in practice** — https://realpython.com/python-magic-methods/

Dunder methods connect a class to Python's own syntax, so `print()`, `len()`, `==`, `+`, indexing and `in` all work on types you define. Next, a way of wrapping behaviour around a function.
