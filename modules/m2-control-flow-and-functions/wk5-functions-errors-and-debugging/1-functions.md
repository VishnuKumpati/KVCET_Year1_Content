# Functions

Every program so far has been a single list of statements, read from top to bottom. When the same work is needed twice, it has to be typed out twice.

Here is a program that checks whether two students have passed:

```python
marks = int(input("Marks: "))
if marks >= 35:
    print("Pass")
else:
    print("Fail")

marks = int(input("Marks: "))
if marks >= 35:
    print("Pass")
else:
    print("Fail")
```

**Output:**

```
Marks: 87
Pass
Marks: 20
Fail
```

The same five lines appear twice. For thirty students they would appear thirty times. Changing the pass mark from 35 to 40 would mean editing thirty copies, and one missed copy would leave the program using two different pass marks.

> **A function is a named, reusable block of code that performs a single, specific task.** It is defined once and executed each time its name is called. A function can accept input values through its parameters and can send a result back to the caller, which allows the same logic to be used many times without being written again.

## Built-in and User-Defined Functions

You have been using functions since your first program. `print()`, `input()`, `int()`, `float()`, `str()` and `range()` are all functions.

```python
age = int(input("Age: "))
print(age)
```

**Output:**

```
Age: 12
12
```

Each of them has a name, and writing that name followed by brackets runs the code behind that name. You have never had to write the steps inside `int()` yourself.

> **A built-in function is a function that Python provides as part of the language, available without being written or imported.** `print()`, `input()`, `int()` and `range()` are built-in functions.
>
> **A user-defined function is a function written by the programmer using the `def` keyword.** Once defined, it is called exactly as a built-in function is: write the name, add brackets, and the code runs.

## Syntax of a User-Defined Function

A function is defined with the `def` keyword, and the rules for writing that definition are these:

- The first line starts with `def`, followed by the function name.
- The name follows the same rules as a variable name, which means lower case with underscores.
- Brackets `()` come after the name.
- The line ends with a colon `:`.
- The statements belonging to the function are indented below it by four spaces.

The shape of a definition is always this:

```
def function_name():
    statements
```

The indented statements are called the **function body**. The colon and the indented block are the same arrangement used by `if` and by `for`.

Here is the smallest function that does something:

```python
def print_result():
    print("Pass")
```

**Output:**

```

```

Nothing was printed. The code contains a `print` statement, and yet it produced no output.

Python read the body and attached it to the name `print_result`. That is all a definition does.

## Definition and Call

Two separate things are needed to get output from a function, and they are easy to confuse.

| | What it looks like | What it does |
| --- | --- | --- |
| Definition | `def print_result():` with an indented body | Stores the body under a name. Runs nothing. |
| Call | `print_result()` | Runs the stored body now. |

A definition is written once, while a call can be written as many times as needed. The definition ends with a colon and has an indented block below it, whereas the call is a plain statement with no colon and no block.

Writing a definition and expecting output is the most common mistake in this topic.

## Calling a Function

To run the body, write the function name followed by brackets. This is called **calling** the function.

```python
def print_result():
    print("Pass")

print_result()
```

**Output:**

```
Pass
```

Three things happened at the line `print_result()`:

1. Python left that line and moved into the function body.
2. Every statement in the body ran, from top to bottom.
3. Python returned to the line straight after the call.

```mermaid
flowchart TD
    A["The call is reached"] --> B["Python moves into<br>the function body"]
    B --> C["Every statement runs<br>top to bottom"]
    C --> D["Python returns to the<br>line after the call"]
```

The call is not indented, so it is not part of the body. The body states what is to be done, and the call is what makes it happen.

## Calling a Function Several Times

One definition serves any number of calls:

```python
def print_result():
    print("Pass")

print_result()
print_result()
```

**Output:**

```
Pass
Pass
```

One `print` statement produced two lines of output, because the body ran in full and then ran in full again.

Here is the program from the start of this topic, written as a function:

```python
def check_result():
    marks = int(input("Marks: "))
    if marks >= 35:
        print("Pass")
    else:
        print("Fail")

check_result()
check_result()
```

**Output:**

```
Marks: 87
Pass
Marks: 20
Fail
```

The output is the same as before, but the code is not. The five lines are written once and used twice, and the pass mark appears in one place only, so changing `35` to `40` changes every call.

A function can also be called inside a loop:

```python
def check_result():
    marks = int(input("Marks: "))
    if marks >= 35:
        print("Pass")
    else:
        print("Fail")

for student in range(3):
    check_result()
```

**Output:**

```
Marks: 87
Pass
Marks: 20
Fail
Marks: 45
Pass
```

Three iterations produced three full runs of the body. Thirty students would need `range(30)`, and nothing else in the program would change.

## Defining Before Calling

Python reads a file from top to bottom, so the definition must be read before the call is reached.

```python
print("Result checker")
check_result()

def check_result():
    print("Pass")
```

**Output:**

```
Result checker
NameError: name 'check_result' is not defined
```

The first line ran. Then Python reached `check_result()` and stopped, because it had not yet read the `def` line. At that moment the name did not exist.

`NameError` is the same error raised when a variable is used before it is assigned. A function name is a name like any other, created at the moment Python reaches its definition.

This is why definitions are written near the top of a file, above the code that calls them.

## Variables Inside a Function

A function body can create variables of its own:

```python
def print_result():
    pass_mark = 35
    print("Pass mark is", pass_mark)

print_result()
```

**Output:**

```
Pass mark is 35
```

`pass_mark` was created inside the body and used on the next line, behaving exactly as a variable does anywhere else in a program.

There is one difference. A variable created inside a function exists only while that function is running, and once the call ends, the name is gone:

```python
def print_result():
    pass_mark = 35
    print("Pass mark is", pass_mark)

print_result()
print(pass_mark)
```

**Output:**

```
Pass mark is 35
NameError: name 'pass_mark' is not defined
```

The function printed its line and finished. The `print` after it failed, because `pass_mark` no longer existed.

This is a benefit rather than a limitation, because a function can name its variables freely without clashing with names used elsewhere in the program. The rules behind it are covered in the topic on variable scope.

## The pass Statement

A function body cannot be empty. Python needs at least one statement after the colon:

```python
def check_result():

print("Checked")
```

**Output:**

```
IndentationError: expected an indented block after function definition on line 1
```

The message names line 1, where the `def` is, because that is the line whose body is missing.

The `pass` statement fills the gap. It is a statement that does nothing, and that is its only purpose:

```python
def check_result():
    pass

check_result()
print("Checked")
```

**Output:**

```
Checked
```

The call ran the body, the body did nothing, and the program carried on.

This is useful while a program is being built. `check_result` can be defined with `pass`, and the loop that calls it thirty times can be written and run straight away. The body is filled in once the rest of the program works.

## Docstrings

> **A docstring is a string written as the first statement inside a function, documenting what that function does.** It is enclosed in triple quotes, and Python stores it in the function's `__doc__` attribute, so it can be read while the program runs.

It is written like this:

```python
def check_result():
    """Ask for a mark and print whether it is a pass."""
    marks = int(input("Marks: "))
    if marks >= 35:
        print("Pass")
    else:
        print("Fail")

print(check_result.__doc__)
```

**Output:**

```
Ask for a mark and print whether it is a pass.
```

Nothing called the function here, so no mark was asked for. The docstring was read straight from the function, because Python keeps it in the function's `__doc__` attribute. Typing `help(check_result)` displays the same text.

A docstring is not a comment, and the two differ in two ways. First, Python discards a comment but keeps a docstring, which is why a docstring can be read by a program or a tool. Second, a comment explains a line to whoever edits the function, whereas a docstring tells whoever calls it what it does, so that they never need to read the body.

Write the first line as one short statement beginning with a verb. For a longer description, leave a blank line after that line and continue below it.

## Further Reading

- **Functions with worked examples** — https://www.programiz.com/python-programming/function
- **Official Python guide to defining functions** — https://docs.python.org/3/tutorial/controlflow.html

So far the brackets after a function name have always been empty. Next, you will learn what goes inside them, so that the same function can work with a different value each time it is called.
