# Nested Conditions

A conditional statement written inside another conditional statement is called a **nested condition**. The inner condition is reached only when Python enters the block that contains it. For an inner `if` inside an outer `if`, the outer condition must therefore be `True` before the inner condition is tested.

## Levels of Indentation

The inner `if` sits inside the outer block, so its lines are indented one level further:

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

Use four spaces for each level of indentation. A nested block is indented one level further than the block containing it, and that indentation is what tells Python which block a line belongs to, so counting levels is how you read nested code:

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

## Nesting Against the and Operator

That first example can also be written with `and`, and it is shorter:

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

So which form is right? It depends on whether the outer condition needs its own outcome.

Use `and` when several conditions together determine one outcome. Here, there is no separate action after checking the marks, so one `if` with `and` is clearer.

Use nesting when the first condition leads to its own action or decision, and another decision has to be made inside that path:

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

Here the outer `if` reports the result and the nested `if` adds something that only makes sense after a fail. A single `and` would not express the same behaviour, because the program needs to handle the first decision and then make another decision inside that path.

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

When both `and` and `or` appear in the same condition, use parentheses to make the intended grouping explicit.

## Nesting Inside else

An earlier example nested inside an `else` rather than an `if`. An `else` block is a block like any other, so it can contain another `if` when the program needs to make a further decision.

Both blocks may nest at once:

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

## Depth and Readability

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

Deep nesting can make code harder to read. When you find yourself several levels in, check whether the logic can be simplified. Combining conditions with `and` is one way to flatten simple cases.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

A conditional statement decides whether a block runs. Next, you will learn how to repeat a block of code, which lets a program work through a whole class of students rather than one.
