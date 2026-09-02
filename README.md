# Python Year 1: Prerequisites and Learning Objectives

A fifteen-week course that takes a learner with no programming background to the point of building, testing, documenting, and publishing a working Python application.

---

## Prerequisites

**Assumed knowledge.** None. The course begins with what a processor executes and why source code must be translated. No prior programming, no computer science background, and no familiarity with the command line is required.

**Required capability.**

- Reading and writing English technical text.
- School-level arithmetic: the four operations, percentages, and averages.
- Basic computer operation: creating folders, saving files, using a web browser.

**Required equipment.**

- A computer running Windows 10 or later, macOS 11 or later, or a current Linux distribution.
- Permission to install software on that computer. Where installation is blocked, a Google account allows the browser-based fallback used in the first module, though the command-line and version-control work requires a local installation.
- An internet connection for downloads, documentation, and the API work in the eleventh through thirteenth weeks.
- Approximately 2 GB of free disk space.

**Software installed during the course.** Python 3.10 or later, Visual Studio Code, and Git. All three are free. Installation is covered step by step in the first week; nothing needs to be prepared in advance.

---

## Course Outcomes

On completion a learner is able to:

- Explain how a Python program is translated and executed, and use that model to diagnose failures.
- Set up and maintain a working Python development environment independently.
- Decompose a stated requirement into pseudocode and then into working code.
- Use conditionals, loops, and functions to express any procedural algorithm at this level.
- Select the appropriate built-in data structure for a problem and manipulate it fluently.
- Model a problem domain with classes, inheritance, and Python's object protocols.
- Read and write files in text, CSV, and JSON form, and organise code across modules and packages.
- Install and isolate third-party dependencies, and consume a public REST API.
- Write automated tests, apply PEP 8, and use a branch-and-pull-request Git workflow.
- Deliver an individual application that is tested, documented, and published to GitHub.

---

## Module Structure

| Module | Title | Weeks | Hours | Coverage |
|---|---|---|---|---|
| M1 | Programming and Computational Thinking | 1-2 | 8 | How code runs, environment, CLI, variables, types, operators, pseudocode, spec-first mindset, Git introduction |
| M2 | Control Flow and Functions | 3-5 | 12 | Conditionals, loops, functions, scope, exceptions, debugging |
| M3 | Built-in Data Structures | 6-8 | 12 | Lists, tuples, dictionaries, sets, strings, comprehensions, generators, data wrangling |
| M4 | Object-Oriented Programming | 9-10 | 8 | Classes, inheritance, polymorphism, dunder methods, decorators, context managers |
| M5 | Production-Ready Python | 11-13 | 12 | Files, modules and packages, venv and pip, requests and APIs, pandas, pytest, PEP 8, Git pull-request workflow, AI-assisted development |
| M6 | Capstone Project | 14-15 | 8 | Build, test, document, and ship an individual application |

Total contact time is 60 hours, delivered as two hours of theory and two hours of laboratory work per week.

---

## Assessment Schedule

| Deliverable | Due | Draws on |
|---|---|---|
| Assignment A1 | Week 3 | Environment, operators, computational thinking, conditionals |
| Assignment A2 | Week 5 | Loops, functions, exception handling, debugging |
| Assignment A3 | Week 8 | Lists, tuples, dictionaries, sets, strings, comprehensions |
| Assignment A4 | Week 10 | Classes, inheritance, polymorphism, dunder methods |
| Assignment A5 | Week 13 | Files, packages, APIs, testing, clean code, Git workflow |
| Capstone project | Week 15 | The full course |

Platform exercises accompany most weeks and are listed in each week's file. They are the minimum practice volume, not an optional extra.

---

## Repository Layout

```
README.md                        this file
navigation.json                  course and topic map used by the learning platform
python-study-guide.md            how to work through the material
python-reference-materials/      supporting reference documents
modules/
    m1-programming-and-computational-thinking/
        wk1-.../                 a week split across more than one file
            1-...md
            2-...md
        wk2-...md                one file per week
    m2-control-flow-and-functions/
    m3-built-in-data-structures/
    m4-object-oriented-programming/
    m5-production-ready-python/
    m6-capstone-project/
```

Each module folder holds one Markdown file per week. Where a week carries more than one file, those files sit in a folder named for the week and are numbered in reading order.
