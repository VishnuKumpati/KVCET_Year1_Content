# Study Guide

This guide describes how to work through the course material. It is short deliberately. Read it once at the start and return to it if progress stalls.

---

## Weekly Working Pattern

Each week is built around four hours of scheduled time and roughly the same again in independent practice. The order matters.

1. **Read the week's file once, without a computer.** The aim is orientation: knowing what the week covers and what will be built.
2. **Work through it a second time at the keyboard, typing every example.** Do not copy and paste. Typing produces the syntax errors that teach syntax.
3. **Complete the laboratory tasks.** These are stated in each week's file and produce a working program, not an exercise fragment.
4. **Solve the platform problems.** These are the minimum practice volume for the week.
5. **Commit the week's work to Git** before moving on.

A week left with unsolved platform problems compounds. Every later week assumes the earlier ones were completed.

---

## Reading Code Against Writing Code

Reading code builds recognition. Writing code builds skill. They are different, and only the second is assessed.

The single most common failure pattern in self-paced programming courses is a learner who understands every example and cannot write a program from a blank file. The remedy is mechanical: after finishing any example, delete it and rewrite it from memory. If that is not possible, the example was read, not learned.

---

## Error Message Discipline

Errors are the normal working condition of programming, not evidence of failure. Two habits make them tractable.

**Read from the bottom.** A Python traceback ends with the error type and message, and the line immediately above names the file and line number where execution stopped. That is the information you need. The intermediate lines describe the path taken to reach the fault and matter only in larger programs.

**Change one thing at a time.** When a program fails, resist editing several lines at once. Change one, run it, observe. Multiple simultaneous changes make it impossible to attribute the result.

Each week's file ends with a table of the errors that week commonly produces, together with their causes and corrections. Consult it before searching the internet.

---

## Use of Documentation

The official Python documentation is the authoritative source and is written to be read, not only searched. From the fifth week onward, look up a function's behaviour in the standard library reference before looking for a tutorial. Documentation literacy is an explicit objective of the twelfth week and an examinable skill.

When consulting external material, verify the Python version it targets. Anything showing `print` without brackets predates 2020 and will not run.

---

## Responsible Use of AI Assistance

AI coding assistants are covered formally in the thirteenth week. Until that point, the following applies.

- Use an assistant to **explain** code, an error message, or a concept.
- Do not use an assistant to **produce** code you then submit. Nothing is learned, and the deficit surfaces in the capstone.
- Any code you did not write must be understood line by line before it enters your repository. You are accountable for explaining it.

---

## Version Control Habit

Git is introduced in the first week and used continuously afterwards. The working cycle is three commands, run whenever something works:

```
git status
git add .
git commit -m "Add temperature converter"
```

Commit when a piece of work functions, not at the end of the week. Small commits with specific messages are what make history useful; a single commit named `update` is not.

---

## Progress Verification

Each week's file ends with a self-assessment section and, where relevant, a completion checklist. Both are intended to be answered without looking back at the material. An item that cannot be answered identifies a section to reread, which is the only reliable signal available in self-paced study.

Do not proceed to a new week with an unresolved checklist item. Programming knowledge is cumulative in a way that most subjects are not: a gap in loops makes every later data-structure topic harder, and a gap in functions makes object-oriented programming close to unintelligible.

---

## Time Allocation Guidance

| Activity | Share of study time |
|---|---|
| Reading and typing examples | 30 percent |
| Laboratory tasks | 30 percent |
| Platform problems | 30 percent |
| Review, self-assessment, and Git work | 10 percent |

If reading exceeds half your time, the balance is wrong. The subject is learned at the keyboard.

---

## Support Resources

Supporting documents accompany this guide:

- **Additional Resources** lists the authoritative primary sources for Python, Git, and the libraries used in the course.
- **Advanced Topics** lists what lies immediately beyond this syllabus, for learners who finish early.
- **Industry Leaders** describes how Python is governed and where it is used in production.
- **Release Notes** records what changed in each Python version, and which versions this course targets.
