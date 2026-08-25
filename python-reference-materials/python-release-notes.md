# Release Notes

Which Python version this course targets, how Python versioning works, and what changed in the releases a learner is likely to encounter.

The authoritative record for any release is its official page at `docs.python.org/3/whatsnew/`. This document summarises only what is relevant at this level.

---

## Version Targeted by This Course

**Python 3.10 or later.** Every example in this course runs unchanged on 3.10 and above. Install whichever version python.org offers by default.

One feature in the syllabus sets that floor: structural pattern matching, the `match` statement covered in the third week, was introduced in Python 3.10 and does not exist before it. Formatted string literals, used from the first week onward, require 3.6 or later, so they impose no practical constraint.

Confirm your version at any time:

```
python --version
```

---

## The Versioning Scheme

Python versions are numbered `3.12.4` in three parts.

- **Major** — the first number. A change here permits incompatible changes to the language. Only one such change has occurred, from 2 to 3.
- **Minor** — the second number. A new minor version is released annually, each October, under the cadence established by PEP 602. New language features arrive here.
- **Patch** — the third number. Bug fixes and security corrections only, with no new features. Upgrading a patch version is always safe.

Each minor release receives approximately two years of full bug-fix support followed by three years of security-only fixes, giving a five-year total lifespan. The current status of every version is published at `devguide.python.org/versions/`.

---

## The Python 2 Boundary

Python 2 and Python 3 were incompatible and coexisted for over a decade. **Python 2 reached its official end of life on 1 January 2020**, and its final release, 2.7.18, appeared in April 2020. It receives no updates of any kind, including security fixes.

This matters when reading material found online. The clearest marker is the print statement:

```python
print "hello"      # Python 2 - raises SyntaxError on any current Python
print("hello")     # Python 3 - correct
```

Any source using the first form predates 2020 and should be closed. Other Python 2 markers include `raw_input()` in place of `input()`, and integer division being the default behaviour of `/`.

---

## Notable Changes by Release

Only items a Year 1 learner may plausibly encounter are listed. Each release contains far more.

### Python 3.9, October 2020

- Dictionary merge and update operators, `|` and `|=`.
- String methods `removeprefix` and `removesuffix`.
- Built-in collection types usable directly in annotations, so `list[int]` replaces the earlier `List[int]`.

### Python 3.10, October 2021

- **Structural pattern matching**, the `match` and `case` statement. This is the feature that sets this course's minimum version.
- Substantially improved error messages, including named suggestions for unclosed brackets and misspelled attributes.
- Union types written with `|`, so `int | None` replaces `Optional[int]`.
- Parenthesized context managers, permitting several resources to be opened across multiple lines in one `with` statement.

### Python 3.11, October 2022

- A large performance improvement. The official documentation reports CPython 3.11 as 10 to 60 percent faster than 3.10 depending on workload, averaging around 1.25 times faster on the standard benchmark suite. No code changes are required to benefit.
- Fine-grained error locations in tracebacks: the failing expression is underlined with carets rather than only the line being named. This measurably shortens debugging and is the strongest single reason to prefer 3.11 or later while learning.
- Exception groups and the `except*` syntax, for handling several simultaneous failures.
- `tomllib` in the standard library, for reading TOML configuration files.

### Python 3.12, October 2023

- Formalised f-string grammar, removing several long-standing restrictions such as reusing the same quote character inside an f-string expression.
- Simplified syntax for generic types and type parameters.
- Further improvements to error message wording and suggestions.

### Python 3.13, October 2024

- A rewritten interactive interpreter with multi-line editing, block-level history, colour output, and direct commands such as `exit` without brackets. This is a visible improvement to the REPL used from the first week.
- An experimental free-threaded build, which can run without the Global Interpreter Lock. Experimental means not enabled in standard installations and not for production use.
- An experimental just-in-time compiler, likewise not enabled by default.

### Python 3.14, October 2025

Released on the annual October cadence. Consult the official *What's New in Python 3.14* page for its complete and accurate change list before relying on any specific feature of this release.

---

## Upgrade Guidance for This Course

**Do not upgrade Python mid-course.** Install once in the first week and stay on that version through the fifteenth. Version changes introduce environment problems that cost time and teach nothing at this stage.

**Patch updates are safe.** Moving from 3.12.4 to 3.12.7 carries no risk.

**Bytecode caches are version-specific.** The `__pycache__` folder contains files named for the version that produced them, such as `helper.cpython-312.pyc`. After any version change, Python regenerates them automatically; deleting the folder is always safe.

**Virtual environments are tied to the interpreter that created them.** From the twelfth week onward, changing Python versions requires recreating any virtual environment rather than reusing it.
