# Packages

Modules divide a program into files. Once there are twenty of them in one directory, the directory has the problem the single file had.

A **package** is a directory containing modules. It groups related modules under one name, so imports say where things come from and the filesystem reflects the structure of the program.

## The Structure

```
shop/
    __init__.py
    products.py
    cart.py
```

`shop` is the package; `products` and `cart` are modules inside it. `__init__.py` marks the directory as a package and runs when the package is first imported.

It may be completely empty, and often is. Its presence is what matters.

Modern Python can treat a directory without `__init__.py` as a package, but the behaviour differs in ways that cause confusing bugs. **Include `__init__.py`.**

## Importing from a Package

The dot separates package from module, and works with every import form:

```python
import shop.cart

print(shop.cart.total(["pen", "book"]))
```

```python
from shop import cart

print(cart.total(["pen", "book"]))
```

```python
from shop.cart import total

print(total(["pen", "book"]))
```

**Output:**

```
260
```

All three reach the same function. The last is usually clearest when one function is used repeatedly.

Note that `import shop.cart` binds the name `shop`, not `cart`, so the full path is needed to call anything. `from shop import cart` binds `cart`.

## Nested Packages

A package can contain packages, each with its own `__init__.py`:

```
shop/
    __init__.py
    products.py
    cart.py
    billing/
        __init__.py
        invoice.py
```

The dots continue:

```python
from shop.billing import invoice

print(invoice.render(["pen", "pen", "book"]))
```

**Output:**

```
3 items: 270
```

Nest only where it earns its place. A package inside a package inside a package makes every import long and every file hard to find. Two levels covers most projects.

## __init__.py as the Public Interface

An empty `__init__.py` is fine. A useful one re-exports the names a user of the package should reach for:

```python
# shop/__init__.py
from shop.products import PRICES
from shop.cart import total
```

Callers then use the package directly:

```python
import shop

print(shop.PRICES)
print(shop.total(["pen", "book"]))
```

**Output:**

```
{'pen': 10, 'book': 250}
260
```

This is what lets `import json` give you `json.dumps` without knowing which file inside the package defines it. It separates the package's **interface** from its **layout**: modules can be split or renamed without breaking anyone, as long as `__init__.py` still exports the same names.

Keep it to imports. `__init__.py` runs on every import of the package, so work done there is work every user pays for.

## Absolute and Relative Imports

Inside a package, a module can name another in two ways.

An **absolute import** gives the full path from the top:

```python
# shop/cart.py
from shop.products import price_of
```

A **relative import** uses dots to mean "relative to this module's package":

```python
# shop/cart.py
from .products import price_of
```

One dot means the current package, two means the parent:

```python
# shop/billing/invoice.py
from ..cart import total
```

Both work. Absolute imports are preferred — they say exactly which module is meant, they read the same wherever they appear, and a file can be moved without the imports silently changing meaning.

Relative imports have one real advantage: renaming the top-level package requires no edits inside it. They are common in libraries for that reason.

Pick one style per project. Mixing them makes the dependencies harder to follow.

## Running a Module in a Package

This catches everyone once. Given `shop/report.py`:

```python
from shop.cart import total

if __name__ == "__main__":
    print("report:", total(["pen", "book"]))
```

Running it as a file fails:

```
python shop/report.py
```

**Output:**

```
ModuleNotFoundError: No module named 'shop'
```

Running it as a module works:

```
python -m shop.report
```

**Output:**

```
report: 260
```

The difference is `sys.path`. Running a **file** puts that file's directory — `shop/` — at the front of the search path, and from inside `shop/` there is no `shop` package to find. Running with **`-m`** puts the current directory there instead, so `shop` is visible.

The rule: **run modules inside a package with `python -m package.module`**, from the project root. `python -m` is also how `pip`, `venv` and `pytest` are invoked, for the same reason.

## A Project Layout

A small project, assembled:

```
project/
    README.md
    requirements.txt
    .gitignore
    shop/
        __init__.py
        products.py
        cart.py
        billing/
            __init__.py
            invoice.py
    tests/
        test_cart.py
    main.py
```

- `shop/` holds the code, as a package.
- `main.py` at the root is the entry point, run with `python main.py`.
- `tests/` sits outside the package, so tests are not shipped with it.
- `README.md` says what the project is and how to run it.
- `requirements.txt` lists what it depends on.

`main.py` stays small — it imports from the package and starts things:

```python
from shop.cart import total

def main():
    print(total(["pen", "book"]))

if __name__ == "__main__":
    main()
```

**Output:**

```
260
```

Running from the project root is what makes `shop` importable. That is why the entry point lives at the root rather than inside the package.

## Modules Against Packages

| | Module | Package |
| --- | --- | --- |
| Is | a `.py` file | a directory |
| Contains | functions, classes, variables | modules and packages |
| Marked by | the `.py` extension | `__init__.py` |
| Imported as | `import module` | `import package.module` |

Start with one file. Split it into modules when it gets unwieldy. Group modules into a package when there are enough of them to need grouping. Each step is a response to a problem, not something to do up front.

## Further Reading

- **Official Python guide to packages** — https://docs.python.org/3/tutorial/modules.html#packages
- **Python modules and packages** — https://realpython.com/python-modules-packages/

A package is a directory with `__init__.py`, imports use dots to walk it, and `python -m` is how a module inside one is run. Next, the packages that come with Python already.
