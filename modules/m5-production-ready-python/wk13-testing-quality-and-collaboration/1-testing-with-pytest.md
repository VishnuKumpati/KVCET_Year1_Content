# Testing with pytest

Programs change all the time. A bug is fixed, a function is improved, or a new feature is added. Each change can accidentally break something that worked before, and the mistake may not be noticed until much later. Checking every part of a program by hand after each change is slow, and it is easy to miss something.

Programmers solve this by writing **tests**: code that checks other code automatically. Once written, tests can be run again after every change, so a broken feature is found immediately.

## Software Testing

> **Software testing is the process of evaluating a program to verify that it behaves as expected and to identify defects before the program is used.**

A **defect**, commonly called a **bug**, is a mistake in a program that makes it behave differently from what was intended. Testing done by a person is called **manual testing**, and testing done by code is called **automated testing**. This topic teaches automated testing.

## Test Cases

Every test is built around a **test case**.

> **A test case is a specific set of conditions used to check one behaviour of a program. It defines the input, the action to perform and the expected result.**

For example, one test case for an `add()` function is:

- **Input:** the numbers `2` and `3`
- **Action:** call `add(2, 3)`
- **Expected result:** the function returns `5`

The value the code really returns when the test runs is called the **actual result**.

- If the actual result matches the expected result, the test **passes**.
- If they differ, the test **fails**. This may indicate a defect in the program or an incorrect expectation in the test.

## Unit Testing

> **A unit is a small, testable part of a program, such as a function, a method or a class.**

> **A unit test checks one small part of a program, usually a function or method, by giving it an input and checking whether it produces the expected result.**

Testing one unit at a time has a clear benefit. When a unit test fails, you know which unit to examine first, instead of searching the whole program.

## pytest

Tests can be written using plain Python code, but a project usually has many tests. Running them one by one and checking each result by hand would be slow. A **testing framework** solves this.

> **A testing framework is a library that provides the structure and tools for writing, organising and running tests, and for reporting their results.**

> **pytest is an external Python testing framework that makes it easy to write small, readable tests. It automatically discovers tests, runs them and gives a detailed report of every test that passes or fails.**

## Project Setup

pytest does not need a special project. In this topic, the test file is kept in the **same folder** as the code it tests, so it can import that code directly.

1. Create a folder named `calculator_project` and open a terminal in it.
2. Create and activate a virtual environment, as explained in **Virtual Environments and pip**.
3. Install pytest:

```
python -m pip install pytest
```

   To confirm the installation, run `python -m pytest --version`. It prints the installed pytest version.

4. Create two files in the folder:

```
calculator_project/
├── calculator.py         # The code to be tested
└── test_calculator.py    # The tests for calculator.py
```

**File: `calculator.py`**

```python
def add(a, b):
    return a + b

class Calculator:
    def multiply(self, a, b):
        return a * b
```

This file contains two units to test: the **function** `add()`, and the **method** `multiply()` of the `Calculator` class. The tests for them are written in a separate file, `test_calculator.py`, so the code being tested stays clean.

## Writing a Test for a Function

**File: `test_calculator.py`**

```python
from calculator import add

def test_add_two_positive_numbers():
    assert add(2, 3) == 5
```

Line by line:

1. `from calculator import add` imports the function being tested. This works because `calculator.py` is in the same folder.
2. `def test_add_two_positive_numbers():` defines a **test function**.
3. `assert add(2, 3) == 5` is the check. It calls `add(2, 3)` and compares the actual result with the expected result, `5`.

> **A test function is a Python function whose name starts with `test`. pytest runs it as a test and reports whether it passed or failed.**

The check is written with the **`assert`** statement.

> **The `assert` statement evaluates a condition. If the condition is `True`, nothing happens and the program continues. If the condition is `False`, Python raises an `AssertionError`.**

So `assert add(2, 3) == 5` means: "the actual result of `add(2, 3)` **must be equal** to `5`." If it is, the test passes. If it is not, an `AssertionError` is raised, and the test fails.

## Test Discovery

pytest finds this test on its own, without being told its name.

> **Test discovery is the process by which pytest automatically searches a project for test files and test functions, using naming conventions.**

By default, pytest uses these naming conventions:

- **Test files** must have names that start with `test_` or end with `_test.py`, such as `test_calculator.py`.
- **Test functions** must have names that start with `test`. The usual style is to start them with `test_`, such as `test_multiply_two_numbers()`.

By default, pytest discovers files and functions that follow these naming conventions. If they do not follow them, pytest may not find them. A test name should also describe exactly what the test checks. Then, when a test fails, its name alone tells you which behaviour is broken.

## Running a Test That Passes

In the terminal, inside the project folder, with the virtual environment active, run:

```
python -m pytest
```

**Possible output:**

```
============================= test session starts ==============================
platform linux -- Python 3.12.3, pytest-9.2.0, pluggy-1.7.0
rootdir: /home/ravi/calculator_project
collected 1 item

test_calculator.py .                                                     [100%]

============================== 1 passed in 0.01s ===============================
```

Here is exactly what pytest did:

1. **Searched the folder** and found `test_calculator.py`, because its name starts with `test_`.
2. **Collected the test**, `test_add_two_positive_numbers()`, because its name starts with `test`. The line `collected 1 item` reports this.
3. **Ran the test function.** `add(2, 3)` returned `5`, so `5 == 5` was `True`, and `assert` raised no error.
4. **Marked the test as passed**, shown by the dot, `.`, after the file name.
5. **Printed a summary**: `1 passed in 0.01s`.

The first lines show your Python version, pytest version and project folder, so they will be different on your computer.

## Running a Test That Fails

To see what happens when code has a defect, change `add()` in `calculator.py` so that it subtracts by mistake:

```python
def add(a, b):
    return a - b
```

Run `python -m pytest` again.

**Possible output (shortened):**

```
test_calculator.py F                                                     [100%]

=================================== FAILURES ===================================
________________________ test_add_two_positive_numbers _________________________

    def test_add_two_positive_numbers():
>       assert add(2, 3) == 5
E       assert -1 == 5
E        +  where -1 = add(2, 3)

test_calculator.py:5: AssertionError
=========================== short test summary info ============================
FAILED test_calculator.py::test_add_two_positive_numbers - assert -1 == 5
============================== 1 failed in 0.01s ===============================
```

Here is exactly what happened:

1. pytest ran the same test function.
2. `add(2, 3)` now returned `-1`, so the condition `-1 == 5` was `False`.
3. `assert` raised an **`AssertionError`**.
4. pytest reports the test as failed with an `F` and continues running the remaining tests.
5. pytest printed a **failure report** explaining what went wrong.

Read the failure report from top to bottom:

- **`F`** after the file name means one test failed.
- **`>`** marks the line where the test failed: `assert add(2, 3) == 5`.
- **`E       assert -1 == 5`** shows the comparison that failed. The actual result was `-1`, and the expected result was `5`.
- **`where -1 = add(2, 3)`** shows the function call that produced the actual result.
- **`test_calculator.py:5`** shows the file and line number of the failing check.

The report shows that `add(2, 3)` returned a different value from what the test expected. This helps you identify where to investigate the problem. Change `add()` back to `return a + b`, run the test again, and it passes.

## Writing a Test for a Method

A **method** is a function that belongs to a class, so it is called on an **object** of that class. This makes testing a method one step longer than testing a function:

| Testing a Function | Testing a Method |
| --- | --- |
| Call the function directly, such as `add(2, 3)` | First **create an object** of the class, then call the method on that object, such as `calculator.multiply(4, 5)` |

Add this test to `test_calculator.py`, and change the import line so it also imports the `Calculator` class:

```python
from calculator import add, Calculator

def test_add_two_positive_numbers():
    assert add(2, 3) == 5

def test_multiply_two_numbers():
    calculator = Calculator()
    assert calculator.multiply(4, 5) == 20
```

- `from calculator import add, Calculator` imports both the function and the class.
- `calculator = Calculator()` creates an object of the `Calculator` class.
- `calculator.multiply(4, 5)` calls the method on that object. The `assert` checks that the actual result is `20`.

Run `python -m pytest -v`. The **`-v`** option, short for **verbose**, lists every test by name:

**Possible output (shortened):**

```
test_calculator.py::test_add_two_positive_numbers PASSED                 [ 50%]
test_calculator.py::test_multiply_two_numbers PASSED                     [100%]

============================== 2 passed in 0.01s ===============================
```

pytest discovered and ran both test functions, one testing a function and one testing a method, and both passed.

## Arrange, Act, Assert

As tests become longer, they need a clear structure. Most tests follow a three-step structure called the **Arrange, Act, Assert (AAA) pattern**.

> **The Arrange, Act, Assert (AAA) pattern is a way of structuring a test in three separate steps: preparing what the test needs, performing the action being tested, and checking the result.**

Each step has one job:

1. **Arrange:** prepare everything the test needs before the action. This includes creating objects, setting up the input values and stating the expected result.
2. **Act:** perform the **one** action being tested, usually by calling a function or method, and store the actual result.
3. **Assert:** compare the actual result with the expected result, using `assert`.

Here is `test_multiply_two_numbers()` rewritten using the AAA pattern:

```python
def test_multiply_two_numbers():
    # Arrange
    calculator = Calculator()
    a = 4
    b = 5
    expected = 20

    # Act
    result = calculator.multiply(a, b)

    # Assert
    assert result == expected
```

- **Arrange** creates the `Calculator` object, sets the inputs `a` and `b`, and records the expected result, `20`.
- **Act** calls `calculator.multiply(a, b)` and stores the actual result in `result`.
- **Assert** checks that `result` equals `expected`.

The comments `# Arrange`, `# Act` and `# Assert` are optional, but they make each step easy to see. This test does exactly the same as the shorter version in **Writing a Test for a Method**. The difference is that each step is now clearly separated.

Very short tests, such as `assert add(2, 3) == 5`, can combine all three steps in one line. For longer tests, keeping the steps separate makes them much easier to read and fix.

## Running Specific Tests

`python -m pytest` runs every test that pytest discovers. You can also choose which tests to run:

- `python -m pytest -v` runs every test and lists each one by name.
- `python -m pytest test_calculator.py` runs only the tests in one file.
- `python -m pytest test_calculator.py::test_multiply_two_numbers` runs only one test function. The `::` separates the file name from the test function name.

## Points to Remember

- **A test case** defines an input, an action and an expected result. A test **passes** when the actual result equals the expected result, and **fails** when it does not.
- **A test function** is a function whose name starts with `test`, and it checks results with `assert`.
- **To test a method**, first create an object of the class, then call the method on that object.
- **The AAA pattern** structures a test as Arrange, Act and Assert, so each step is clear.

## Further Reading

- 📎 **Official pytest getting started guide** — https://docs.pytest.org/en/stable/getting-started.html
- 📎 **Official pytest guide to writing assertions** — https://docs.pytest.org/en/stable/how-to/assert.html

The tests in this topic check a few typical inputs. But what happens with zero, empty values, very large numbers or inputs a function should reject? In the next topic, **Edge Cases**, you'll learn how to identify and test these situations, including how to check that a function raises an error and how to test many inputs with one test function.
