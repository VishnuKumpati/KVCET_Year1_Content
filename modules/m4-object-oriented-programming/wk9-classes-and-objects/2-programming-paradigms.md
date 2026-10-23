# Programming Paradigms

Think about how a college could organise its annual event. One way is to write a single list of steps and follow it from top to bottom. Another way is to form teams, such as a stage team and a food team, where each team looks after its own work. A third way is to set up stations, where each station takes something in and hands back a result, without disturbing anything else.

All three approaches can run the same event. They simply organise the work differently. Programs work the same way. Object-oriented programming is one way to organise a program, and there are others. Each way of organising a program is called a **programming paradigm**.

> **A programming paradigm is a style or approach to organising a program.**

This topic compares three common paradigms: **procedural**, **object-oriented** and **functional** programming.

## The Three Paradigms

| Paradigm | Main Idea | Event Analogy |
| --- | --- | --- |
| **Procedural** | Organises a program as steps and procedures that perform tasks. | One list of steps, followed in order |
| **Object-Oriented (OOP)** | Organises a program around objects that combine data with related behaviour. | Teams that manage their own responsibilities |
| **Functional** | Organises a program around functions that transform inputs into outputs, often avoiding changes to existing data. | Stations that take something in and produce a result |

## One Task in Three Styles

Each example below does the same task: **add 5 bonus marks**.

### Procedural Style

```python
marks = [87, 72, 95]

for position in range(len(marks)):
    marks[position] = marks[position] + 5

print(marks)
```

**Output:**

```
[92, 77, 100]
```

**What makes this procedural:**

- The program is a **sequence of steps** that runs from top to bottom.
- The data (`marks`) and the steps that work on it are **separate**.
- The steps **change the original list** directly.

### Object-Oriented Style

```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

    def add_bonus(self):
        self.marks += 5

students = [
    Student("Ravi", 87),
    Student("Priya", 72),
    Student("Arun", 95)
]

for student in students:
    student.add_bonus()
    print(student.name, student.marks)
```

**Output:**

```
Ravi 92
Priya 77
Arun 100
```

**What makes this object-oriented:**

- Each `Student` object keeps **its own data** (`name` and `marks`) together with **the method that works on it** (`add_bonus()`).
- The program does not change the marks itself. It **asks each object** to update its own marks.

The class syntax used here is explained step by step in the topics that follow, starting with **Classes and Objects**.

### Functional Style

```python
def add_bonus(marks):
    return [mark + 5 for mark in marks]

marks = [87, 72, 95]
updated = add_bonus(marks)

print(marks)
print(updated)
```

**Output:**

```
[87, 72, 95]
[92, 77, 100]
```

**What makes this functional:**

- The work is done by a **function** that takes the marks as input and **returns a new list** as output.
- `[mark + 5 for mark in marks]` builds a new list of increased marks.
- The original list is **not changed**, which is why `marks` still prints `[87, 72, 95]`.

A function like this is called a **pure function**: it gives the same output for the same input, and it does not change anything outside itself.

## Pros and Cons

| Paradigm | Pros | Cons |
| --- | --- | --- |
| **Procedural** | Simple and easy to follow for small programs | Becomes hard to manage as the program grows, because many steps may change the same data |
| **Object-Oriented** | Keeps related data and actions together, and makes large programs easier to organise and reuse | Needs more planning and more code for very small tasks |
| **Functional** | Pure functions are predictable and easier to test, because the same inputs produce the same outputs without changing anything outside the function. | Can feel unfamiliar at first, and creating new data rather than modifying existing data can use more memory. |

## Choosing a Paradigm

| Situation | A Good Choice |
| --- | --- |
| A short script that does a few steps once | **Procedural** |
| A program that models real things, such as students, accounts or products, and keeps growing | **Object-Oriented** |
| A task that mainly transforms data, such as calculating new values from a list | **Functional** |

Python supports **all three paradigms**, and real programs often **mix them**. For example, an object-oriented program can still use functional-style functions inside it. **There is no single best paradigm.** The right choice depends on the size and type of the problem.

## Further Reading

- 📎 **Official Python guide to functional programming** — https://docs.python.org/3/howto/functional.html
- 📎 **Official Python tutorial on classes** — https://docs.python.org/3/tutorial/classes.html

This week focuses on the object-oriented paradigm. In the next topic, **Classes and Objects**, you'll learn how to create your own classes and objects in Python.
