# Nested Conditions

A conditional statement written inside another conditional statement is called a **nested condition**. The inner condition is reached only when Python enters the block that contains it. For an inner `if` inside an outer `if`, the outer condition must therefore be `True` before the inner condition is tested.

## Syntax of a Nested Condition

A nested condition is written in the same form as any conditional statement. What makes it nested is where it sits:

```
if outer_condition:
    statements
    if inner_condition:
        statements
```

The inner `if` is one of the statements in the outer block, so it is indented one level further. Use four spaces for each level. That indentation is what tells Python which block a line belongs to, so counting levels is how you read nested code.

Here is that form filled in:

```python
marks = 87
attendance = 90
if marks >= 35:
    print("Passed the exam")
    if attendance >= 75:
        print("Eligible for the prize")
```

**Output:**

```
Passed the exam
Eligible for the prize
```

The outer condition was `True`, so its block ran. Inside that block, the inner condition was tested and also came out `True`.

The important idea is that the outer condition controls whether Python ever reaches the inner condition:

```
marks >= 35 ?
      ↓ True
attendance >= 75 ?
```

The attendance check is not made independently. Python reaches it only after the marks condition is `True`.

```mermaid
flowchart TD
    A{"marks >= 35"} -->|False| B(["End"])
    A -->|True| C[\"Print Passed"\]
    C --> D{"attendance >= 75"}
    D -->|True| E[\"Print Eligible"\]
    D -->|False| B
    E --> B
```

Fail the exam and the attendance is never looked at:

```python
marks = 20
attendance = 90
if marks >= 35:
    print("Passed the exam")
    if attendance >= 75:
        print("Eligible for the prize")
```

**Output:**

```

```

Nothing printed. The outer condition was `False`, so the entire block was skipped, inner condition and all.

## Nesting Inside else

An `else` block is a block like any other, so it can contain a nested condition when a further decision has to be made along that path:

```python
marks = 32
if marks >= 35:
    print("Passed the exam")
else:
    print("Failed the exam")
    if marks >= 30:
        print("You may sit the retest")
```

**Output:**

```
Failed the exam
You may sit the retest
```

The outer condition was `False`, so the `else` block ran. Inside it, the nested condition asked a second question that only makes sense after a fail.

Both blocks of an `if`/`else` statement may hold nested conditions at once:

```python
marks = 95
if marks >= 35:
    print("Passed")
    if marks >= 90:
        print("With distinction")
else:
    print("Failed")
    if marks >= 30:
        print("Retest allowed")
```

**Output:**

```
Passed
With distinction
```

Read this by indentation, not by order. The `if` and the `else` are at the outer level. Each holds a `print` and a nested `if` at the inner level. The second inner `if` belongs to the `else` because it is indented inside that block.

## Nested Conditions and Logical Operators

When only the final outcome matters, two conditions can be joined with the `and` operator and tested in a single `if`:

```python
marks = 87
attendance = 90
if marks >= 35 and attendance >= 75:
    print("Eligible for the prize")
```

**Output:**

```
Eligible for the prize
```

So which form is right? It depends on whether the outer condition needs an outcome of its own.

Use the `and` operator when several conditions together determine one outcome. Here, nothing happens as a result of the marks alone, so a single `if` is clearer.

Use a nested condition when the first condition leads to its own action, and another decision has to be made along that path. The retest example above cannot be written with `and`, because the program reports the failure first and then decides separately whether a retest is allowed.

## Grouping Conditions with Parentheses

A condition can contain both `and` and `or`. Python evaluates `and` before `or`, which can change the result if you are not careful. Use parentheses when you want to make the intended grouping explicit:

```python
student_class = 9
attendance = 40
if (student_class == 9 or student_class == 10) and attendance > 75:
    print("May join the trip")
else:
    print("Not in the trip group")
```

**Output:**

```
Not in the trip group
```

The parentheses make Python evaluate the `or` expression first. The student must be in class 9 or 10, and then must also have attendance above 75 percent:

```
(class 9 OR class 10) AND attendance > 75
```

Without them, Python would read the condition as "class 9, or class 10 with good attendance". Class 9 would match on its own, the attendance would never be checked, and a student with 40 percent would be told to join the trip.

## Readability of Nested Code

Python allows nesting to any depth, but deep nesting can make code harder for humans to read:

```python
marks = 87
attendance = 90
fees_paid = True
if marks >= 35:
    if attendance >= 75:
        if fees_paid:
            print("Eligible for the prize")
```

**Output:**

```
Eligible for the prize
```

`fees_paid` already holds a Boolean value, either `True` or `False`, so Python can use the variable directly as a condition. There is nothing left to compare it against.

Three levels deep, the actual work is already buried inside several layers of indentation. Nothing here needs separate outcomes, so `and` says it better:

```python
marks = 87
attendance = 90
fees_paid = True
if marks >= 35 and attendance >= 75 and fees_paid:
    print("Eligible for the prize")
```

**Output:**

```
Eligible for the prize
```

When you find yourself several levels in, check whether the logic can be simplified. Combining conditions with `and` is one way to flatten simple cases.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

A conditional statement decides whether a block runs. Next, you will learn how to repeat a block of code, which lets a program work through a whole class of students rather than one.
