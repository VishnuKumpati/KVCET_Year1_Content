# The `while` Loop

A `for` loop works through a sequence one item at a time. It stops when there are no more items to process.

Some problems are not like that. Suppose a program asks for a password and must keep asking until the right one is typed. The person might get it right on the first attempt, or the fourth, or the tenth. There is no sequence to walk through.

## Syntax of a while Loop

A `while` loop is a statement that repeats a block for as long as its condition evaluates to `True`. The condition is tested before every iteration, and the loop ends at the first test that returns `False`.

Every `while` loop is written in the same form:

```
while condition:
    statements
```

Two parts make it up:

- the keyword `while`, followed by a condition
- a colon `:`, then an indented block

That is the same shape as an `if` statement. The difference is what happens after the block finishes. An `if` moves on. A `while` goes back and tests the condition again:

```mermaid
flowchart TD
    A["Check the condition"] --> B{"Is it True?"}
    B -->|Yes| C["Run the<br>indented block"]
    C --> A
    B -->|No| D(["Loop ends"])
```

That single arrow going back is the whole idea. `if` checks once. `while` checks again after every iteration.

Here is that form filled in:

```python
count = 1
while count <= 3:
    print(count)
    count += 1
```

**Output:**

```
1
2
3
```

Follow it value by value:

```
count = 1   →   1 <= 3 is True    →   print 1, count becomes 2
count = 2   →   2 <= 3 is True    →   print 2, count becomes 3
count = 3   →   3 <= 3 is True    →   print 3, count becomes 4
count = 4   →   4 <= 3 is False   →   loop ends
```

The condition was tested four times and the block ran three times. The last test is what stops the loop.

## Solving the Password Problem

Now the problem this topic opened with:

```python
password = ""
while password != "python123":
    password = input("Enter password: ")
print("Access granted")
```

**Output:**

```
Enter password: hello
Enter password: python123
Access granted
```

The condition is tested before the first iteration, so `password` has to exist by then. It starts as an empty string, which does not match the expected password, so the condition is `True` and the loop runs.

Each iteration replaces `password` with whatever was typed. When the typed value matches, the condition becomes `False` and the loop ends. The `print` after it is outside the block, so it runs once, after the loop is over.

Nothing in this program says how many attempts to allow, because nothing needs to. The condition decides.

Because the condition is tested before the first iteration, a `while` loop may run many times, once, or not at all. A condition that is already `False` skips the block entirely.

## Updating the Condition

A `while` loop evaluates the same condition on every iteration. For the loop to end, at least one statement in the block must change a value that the condition depends on.

In the password loop, that job belongs to one line:

```python
password = input("Enter password: ")
```

The condition tests `password`, and this line replaces `password` on every iteration. Each pass gives the condition a new value to judge, and the loop ends as soon as the typed value matches.

The updated value does not have to come from the person. It can be worked out inside the loop:

```python
total = 0
number = 1
while total < 10:
    total += number
    number += 1
print("Total:", total)
```

**Output:**

```
Total: 10
```

The condition tests `total`, and `total += number` raises it on every iteration. The loop added 1, then 2, then 3, then 4, reaching 10 and stopping there. Nobody typed anything.

A **counter** is a variable that records how many iterations have run, updated by a fixed amount on each pass. `count += 1` in the first example is a counter, and so is `number += 1` here. The password loop has no counter at all, which shows that counting is one way to move a condition towards `False` and not a requirement of the statement.

Before running any `while` loop, check two things. Can the condition become `False`, and does something in the block move it in that direction?

## Infinite Loops

An **infinite loop** is a loop whose condition never becomes `False`, so it repeats without end. In a `while` loop this happens when no statement in the block changes any value the condition depends on. Every test then evaluates the same expression against the same values and returns the same result.

Here is the counting loop with one line removed:

```python
count = 1
while count <= 3:
    print(count)
```

**Output:**

```
1
1
1
1
1
...
```

The output continues without stopping.

Nothing changes `count`, so every test asks the same question and receives the same answer:

```
1 <= 3 is True
1 <= 3 is True
1 <= 3 is True
```

It is not an error, so Python reports nothing. The program simply never finishes.

To stop one, press **Ctrl+C** in the terminal. That interrupts the running program and returns you to the command line.

The fix is the line that was removed:

```python
count = 1
while count <= 3:
    print(count)
    count += 1
```

**Output:**

```
1
2
3
```

## Choosing Between for and while

Both loops repeat a block, and either can often be made to do the job. The question is which one fits the problem.

| Loop | Suits |
| --- | --- |
| `for` | walking through a sequence, or repeating a known number of times |
| `while` | repeating as long as a condition remains `True` |

Printing a welcome for fifty students is a `for` loop, because you have a sequence of fifty student numbers to process. Asking for a password is a `while` loop, because the loop ends when the condition changes, and the number of attempts is not known in advance.

Treat this as a guideline rather than a rule. Where both would work, pick the one that reads more clearly.

## Further Reading

- **Official Python guide to control flow** — https://docs.python.org/3/tutorial/controlflow.html

Both loop statements are now in place. Next, you will use them to work through a grid, where every row has several positions across it.
