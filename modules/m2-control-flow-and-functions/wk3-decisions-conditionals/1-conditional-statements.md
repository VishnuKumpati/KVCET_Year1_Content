# Conditional Statements

Every condition you have written so far only produced a value. `marks >= 35` returned `True` or `False`, and the program carried on regardless. A **conditional statement** uses that result to decide which lines of code run and which are skipped.

The `if` does not create the condition or work out a new kind of value. It simply tells Python to act on the result of the condition.

## The if Statement

The `if` statement runs a block of code only when its condition is `True`.

```python
marks = 87
if marks >= 35:
    print("Pass")
```

**Output:**

```
Pass
```

Read that the way Python does. The condition is evaluated first, and its value decides the rest:

```mermaid
flowchart LR
    A["marks >= 35"] --> B["87 >= 35"]
    B --> C["True"]
    C --> D["The block runs"]
```

Three things make up the statement:

- the keyword `if`, followed by a condition
- a colon `:` at the end of the line
- one or more indented statements below it, which run only when the condition is `True`

Change the mark and the indented line is skipped entirely:

```python
marks = 20
if marks >= 35:
    print("Pass")
```

**Output:**

```

```

Nothing was printed. The condition was `False`, so Python passed over the indented line without running it.

```mermaid
flowchart TD
    A(["Start"]) --> B{"marks >= 35"}
    B -->|True| C[\"Print Pass"\]
    B -->|False| D(["End"])
    C --> D
```

## Blocks and Indentation

The indented statements under an `if` are called a **block**. A block can hold as many statements as you need, as long as every one of them is indented by the same amount:

```python
marks = 87
if marks >= 35:
    print("Pass")
    print("Well done")
print("Result printed")
```

**Output:**

```
Pass
Well done
Result printed
```

The first two lines are indented, so both belong to the block and both ran. The third line is not indented, so it is outside the block. It runs whatever the condition decided.

Indentation is not decoration in Python. It is the only thing that marks where a block starts and ends. Leave it out and the program will not run at all:

```python
marks = 87
if marks >= 35:
print("Pass")
```

**Output:**

```
IndentationError: expected an indented block after 'if' statement on line 2
```

`IndentationError` is the name Python gives this error, and the message names the line that needed a block.

Use four spaces for one level of indentation. This is the standard Python convention and is what the Python community recommends. Python itself does not demand exactly four. What matters is that statements belonging to the same block use the same indentation level. Four spaces is simply the amount everyone agrees on, and most code editors can be set to insert it when you press Tab.

## The else Clause

An `if` on its own does one thing or nothing. Adding an `else` clause gives a second block, which runs when the condition is `False`:

```python
marks = 20
if marks >= 35:
    print("Pass")
else:
    print("Fail")
```

**Output:**

```
Fail
```

The condition was `False`, so the `if` block was skipped and the `else` block ran instead.

In an `if`/`else` statement, exactly one of the two blocks runs. Never both, never neither.

```mermaid
flowchart TD
    A{"marks >= 35"} -->|True| B[\"Print Pass"\]
    A -->|False| C[\"Print Fail"\]
    B --> D(["End"])
    C --> D
```

The `else` keyword takes no condition of its own. It simply catches everything the `if` did not.

## The elif Clause

Two paths are often not enough. A mark might deserve a grade rather than a pass or a fail, and that needs several conditions tested in turn.

The `elif` clause does this. The name is short for "else if", and you can use as many as you need. That gives three keywords, each with one job:

| Keyword | What it does |
| --- | --- |
| `if` | Starts the decision and tests the first condition |
| `elif` | Tests another condition, but only if every condition above it was `False` |
| `else` | Runs when none of the conditions above it were `True`, and tests nothing itself |

```python
marks = 72
if marks >= 90:
    print("Distinction")
elif marks >= 60:
    print("First class")
elif marks >= 35:
    print("Pass")
else:
    print("Fail")
```

**Output:**

```
First class
```

Python worked down the chain and stopped at the first condition that was `True`:

```mermaid
flowchart TD
    A{"marks >= 90"} -->|True| A1[\"Print Distinction"\]
    A -->|False| B{"marks >= 60"}
    B -->|True| B1[\"Print First class"\]
    B -->|False| C{"marks >= 35"}
    C -->|True| C1[\"Print Pass"\]
    C -->|False| D1[\"Print Fail"\]
    A1 --> E(["End"])
    B1 --> E
    C1 --> E
    D1 --> E
```

`72 >= 90` was `False`, so Python moved on. `72 >= 60` was `True`, so that block ran and the whole chain ended there. Follow the diagram and you leave it at `First class`, never reaching the two diamonds below. The remaining `elif` and the `else` were never even tested, although `72 >= 35` is also `True`.

That is the rule to hold on to. Only the first matching block runs, and everything below it is skipped.

Three rules govern how a chain is built.

- The `if` comes first, and a chain has exactly one.
- Any number of `elif` clauses may follow it.
- The `else` is optional and must come last. Leave it out and a mark that matches nothing produces no output at all, exactly as a lone `if` does.

## Independent if Statements

An `if`/`elif`/`else` chain runs at most one block. Separate `if` statements are not a chain, and Python tests each one on its own, so more than one of them can run:

```python
marks = 95

if marks >= 35:
    print("Pass")

if marks >= 90:
    print("Distinction")
```

**Output:**

```
Pass
Distinction
```

Both conditions were `True`, so both blocks ran. Nothing joins the two statements, so the result of the first has no bearing on whether the second is tested.

Now the same two conditions, in the same order, joined into a chain by one word:

```python
marks = 95

if marks >= 35:
    print("Pass")
elif marks >= 90:
    print("Distinction")
```

**Output:**

```
Pass
```

One line instead of two. The `elif` made the second condition depend on the first, so `95 >= 35` matched, the chain ended, and `95 >= 90` was never tested.

| Form | How Python treats it |
| --- | --- |
| Separate `if` statements | Every condition is tested, and every matching block runs |
| One `if` with `elif` | Conditions are tested from the top, and the first match ends the chain |

So the number of blocks that run is decided by how the statements are written, not by how many conditions happen to be `True`.

## Order of Conditions

The conditions in a chain do not have to be mutually exclusive. Several of them can be `True` at the same time, and Python makes no attempt to find the best match. It takes the first condition that is `True` and skips the rest.

So the order of the conditions can change the result. Put them in the wrong order and the program runs perfectly while giving the wrong answer:

```python
marks = 95
if marks >= 35:
    print("Pass")
elif marks >= 90:
    print("Distinction")
```

**Output:**

```
Pass
```

A mark of 95 deserves a distinction. It got `Pass`, because `95 >= 35` was tested first and matched. The `elif` below it never had a chance to run.

Python reported no error here, and that is what makes this dangerous. The code is valid. Only the logic is wrong.

```python
marks = 95
if marks >= 90:
    print("Distinction")
elif marks >= 35:
    print("Pass")
```

**Output:**

```
Distinction
```

The rule that follows is short. When conditions overlap, put the more specific condition first and the more general condition later:

```
marks >= 90     ← the more specific condition, so it goes first
marks >= 35     ← the more general condition, so it goes after
```

`marks >= 90` is the more specific of the two because fewer marks satisfy it. Every mark that passes it also passes `marks >= 35`, so testing the general one first would leave the specific one unreachable. Grades therefore run from the highest mark down, ages from the oldest band down, prices from the largest discount down.

## Further Reading

- **if, elif and else with worked examples** — https://www.programiz.com/python-programming/if-elif-else
- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

Every condition here has been a single comparison. Next, you will place one conditional statement inside another, so that a second decision depends on the answer to the first.
