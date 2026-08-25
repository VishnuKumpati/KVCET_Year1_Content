# Advanced Topics

Material that lies immediately beyond this syllabus. Nothing here is examined in Year 1. It is listed for two purposes: to give learners who finish early a defensible direction, and to name the concepts a learner will encounter in real code before the course has taught them.

Each entry states what the topic is, why it exists, and the official page that documents it.

---

## Type Annotations and Static Checking

Python does not require variable types to be declared, and does not check them at run time. Annotations allow them to be stated anyway, so that tools and readers can verify intent before the program runs.

```python
def average(marks: list[float]) -> float:
    return sum(marks) / len(marks)
```

Python itself ignores these annotations. A separate tool such as mypy reads them and reports inconsistencies. Annotations appear throughout professional Python codebases and in most library documentation, which is why a learner should recognise the syntax even before using it.

Reference: `docs.python.org/3/library/typing.html`, PEP 484, and `mypy.readthedocs.io`.

---

## Data Classes and Structured Records

Classes written purely to hold data require a substantial amount of repeated code. The `dataclasses` module generates that code from a declaration.

```python
from dataclasses import dataclass

@dataclass
class Student:
    name: str
    marks: list[float]
```

This produces an initializer, a readable representation, and equality comparison automatically. It is the standard modern approach for record-shaped classes and directly extends the object-oriented material of the ninth and tenth weeks.

Reference: `docs.python.org/3/library/dataclasses.html`.

---

## Path Handling with pathlib

File paths written as strings break across operating systems and require awkward joining. The `pathlib` module represents a path as an object with methods.

```python
from pathlib import Path

data = Path("reports") / "2026" / "summary.csv"
print(data.exists(), data.suffix)
```

This is the recommended approach for all new code and supersedes much of the older `os.path` material, which is nonetheless still covered in the eleventh week because it remains widespread.

Reference: `docs.python.org/3/library/pathlib.html`.

---

## Functional Tools in itertools and functools

The standard library contains two modules of tools for working with iteration and functions: `itertools` for combining, grouping, and slicing iterables, and `functools` for caching results, reducing sequences, and partially applying functions. Both produce shorter and faster solutions to problems that would otherwise require nested loops.

Reference: `docs.python.org/3/library/itertools.html` and `docs.python.org/3/library/functools.html`.

---

## Concurrency and the Global Interpreter Lock

A program that spends its time waiting, for a network response or a disk read, can do other work while it waits. Python offers three distinct mechanisms, and choosing between them requires understanding a constraint specific to CPython.

- **Threading** runs multiple threads in one process. CPython's Global Interpreter Lock permits only one thread to execute Python bytecode at a time, so threading helps with waiting and does not help with computation.
- **Multiprocessing** runs separate processes, each with its own interpreter and therefore its own lock, which does accelerate computation across processor cores at the cost of heavier communication between them.
- **Asynchronous programming** with `async` and `await` runs many waiting operations in a single thread through cooperative scheduling. It is the standard approach for network-heavy code.

Work to make the lock optional began with an experimental free-threaded build introduced in Python 3.13.

Reference: `docs.python.org/3/library/asyncio.html`, `docs.python.org/3/library/threading.html`, and PEP 703.

---

## Packaging and Distribution

The eleventh week covers organising code into modules and packages within one project. Distributing that project so others can install it with pip is a further step, configured through a `pyproject.toml` file that declares the project's name, version, and dependencies.

Reference: `packaging.python.org`.

---

## Performance Measurement

Optimising code without measuring it first is guesswork. Two standard-library tools remove the guesswork: `timeit` measures how long a small operation takes, averaged over many repetitions, and `cProfile` reports where a whole program spent its time, function by function.

Reference: `docs.python.org/3/library/timeit.html` and `docs.python.org/3/library/profile.html`.

---

## Descriptors, Metaclasses and the Data Model

The mechanisms that make properties, methods, and class creation work are themselves written in Python and can be customised. This is the machinery underneath the decorators and dunder methods of the tenth week. It is rarely needed in application code and frequently used in libraries and frameworks, which is why reading framework source requires it.

Reference: `docs.python.org/3/reference/datamodel.html`.

---

## Continuous Integration

The thirteenth week introduces automated tests run locally. Continuous integration runs those tests automatically on a server whenever code is pushed, so that a defect is reported before a pull request is merged. GitHub Actions is the common implementation and is configured with a YAML file inside the repository.

Reference: `docs.github.com/actions`.

---

## Suggested Order of Study

For a learner working ahead, the sequence below builds in the order that each topic supports the next.

1. Type annotations, then data classes.
2. pathlib, then itertools and functools.
3. Automated testing beyond the basics, then continuous integration.
4. Packaging and distribution.
5. Asynchronous programming.
6. The data model, descriptors, and metaclasses.

Concurrency and the data model are placed last deliberately. Both are frequently attempted early and are the two topics most likely to produce confident misunderstanding.
