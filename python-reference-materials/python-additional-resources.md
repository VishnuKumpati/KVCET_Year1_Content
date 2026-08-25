# Additional Resources

Primary sources only. Every entry below is the official documentation for the tool it describes, maintained by the people who build it. Prefer these over any secondary tutorial; where a secondary source is listed, it is marked as such and included for a specific reason.

---

## Python Language and Standard Library

| Resource | Address | Use |
|---|---|---|
| Official documentation home | docs.python.org/3/ | Entry point for everything below |
| The Python Tutorial | docs.python.org/3/tutorial/ | The authoritative guided introduction, written by the core developers |
| Standard Library Reference | docs.python.org/3/library/ | Definitive behaviour of every built-in function, type, and standard module |
| Language Reference | docs.python.org/3/reference/ | Formal grammar and semantics; consult for precise rules |
| Built-in Functions | docs.python.org/3/library/functions.html | The functions available without any import |
| Downloads | python.org/downloads/ | The only source from which to install Python |
| Version status | devguide.python.org/versions/ | Which Python versions are supported and until when |

---

## Style and Language Design Documents

Python Enhancement Proposals are the documents in which the language is specified and its conventions agreed. Four are directly relevant at this level.

| Document | Address | Subject |
|---|---|---|
| PEP 8 | peps.python.org/pep-0008/ | Style Guide for Python Code; the naming and formatting standard used course-wide |
| PEP 20 | peps.python.org/pep-0020/ | The Zen of Python; the design principles behind the language, also printed by `import this` |
| PEP 257 | peps.python.org/pep-0257/ | Docstring Conventions; how to document functions, classes, and modules |
| PEP 484 | peps.python.org/pep-0484/ | Type Hints; beyond this syllabus but encountered in real code from the eleventh week onward |

---

## Editor and Environment

| Resource | Address | Use |
|---|---|---|
| Visual Studio Code Python tutorial | code.visualstudio.com/docs/python/python-tutorial | Official setup and usage guide for the editor used in this course |
| Python extension documentation | code.visualstudio.com/docs/languages/python | Interpreter selection, debugging, and testing integration |
| Google Colab | colab.research.google.com | Browser-based fallback where local installation is not possible |
| venv documentation | docs.python.org/3/library/venv.html | Virtual environments, introduced in the twelfth week |
| pip documentation | pip.pypa.io/en/stable/ | Installing and managing third-party packages |
| Python Package Index | pypi.org | The public repository from which pip installs |

---

## Version Control

| Resource | Address | Use |
|---|---|---|
| Pro Git, second edition | git-scm.com/book/en/v2 | The standard Git reference, available free in full; chapters 1 to 3 cover this course's needs |
| Git reference manual | git-scm.com/doc | Command-by-command documentation |
| Git downloads | git-scm.com/downloads | Installation |
| GitHub Docs | docs.github.com | Remote repositories, pull requests, and branch protection |

---

## Libraries Used in Later Modules

| Library | Documentation | Introduced |
|---|---|---|
| requests | requests.readthedocs.io | Week 12, for consuming REST APIs |
| pandas | pandas.pydata.org/docs/ | Week 12, at an introductory level |
| pytest | docs.pytest.org | Week 13, for automated testing |

The pandas documentation includes a section titled *10 minutes to pandas*, which is the recommended starting point for the light coverage this course requires.

---

## Secondary Resources with a Specific Purpose

Two non-official tools are worth naming because they solve problems the official documentation does not.

**Python Tutor** at pythontutor.com executes a short program and draws every variable, list, and object at each step. For learners who cannot yet picture what a loop or a reference is doing, this makes the invisible visible. It is limited to small programs, which is exactly the size used in the first eight weeks.

**Real Python** at realpython.com publishes long-form tutorials that are technically reliable and version-labelled. Treat it as a supplement when an official page is too terse, never as a replacement for the standard library reference.

---

## Source Evaluation

Programming material dates quickly. Before trusting an external page, apply three checks.

- **Version.** Any example using `print` without brackets is Python 2 and predates 2020. Discard it.
- **Date.** Prefer material published or revised within the last three years, particularly for packaging, virtual environments, and type hints, where practice has changed.
- **Executability.** Run the example. A page whose code does not run as printed is not a source worth continuing with.
