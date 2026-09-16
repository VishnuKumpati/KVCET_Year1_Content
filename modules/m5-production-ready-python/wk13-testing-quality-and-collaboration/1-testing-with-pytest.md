# Testing with pytest

Testing so far has meant running the program and looking at the output. That works for ten lines and fails for a hundred, for three reasons.

**It does not scale.** Checking every path by hand after every change takes longer than the change did.

**It is not repeated.** Code that worked last week is assumed to still work. Usually it does. The times it does not are called **regressions**, and they are found by users.

**It is not recorded.** The cases you thought about exist only in your head, and are gone next month.

An **automated test** is code that checks other code. It runs in a second, checks everything every time, and is a permanent record of what the code is supposed to do.

## assert

`assert` is the building block. It does nothing if its condition is true, and raises `AssertionError` if it is false:

```python
assert 2 + 2 == 4
print("first check passed")
assert 2 + 2 == 5
print("never reached")
```

**Output:**

```
first check passed
AssertionError
```

That is the whole idea of a test: state what should be true, and let it fail loudly when it is not.

## pytest

`unittest` is built in; **pytest** is what most projects use, because a test is an ordinary function with an ordinary `assert`.

```
pip install pytest
```

Given `calculator.py`:

```python
def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("cannot divide by zero")
    return a / b

def average(numbers):
    return sum(numbers) / len(numbers)
```

Write `test_calculator.py` beside it:

```python
from calculator import add, divide

def test_add_two_positives():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, 1) == 0

def test_divide():
    assert divide(10, 2) == 5.0
```

No class, no special assertion methods, no setup. Three functions with three asserts.

## Naming Matters

pytest finds tests by name, and the conventions are requirements:

- test **files** are named `test_*.py` or `*_test.py`
- test **functions** are named `test_*`
- test **classes**, if used, are named `Test*`

A function called `check_add` is not collected and never runs. A test that silently does not run is worse than no test, because it looks like coverage.

## Running

Run `pytest` from the project directory. It finds the files, finds the functions, and runs them:

```
pytest
```

**Output:**

```
============================= test session starts =============================
platform win32 -- Python 3.13.11, pytest-9.1.1, pluggy-1.6.0
rootdir: C:\Users\anita\project
collected 3 items

test_calculator.py ...                                                   [100%]

============================== 3 passed in 0.01s ==============================
```

Each dot is a passing test. `-q` gives the short form:

```
pytest -q
```

**Output:**

```
...                                                                      [100%]
3 passed in 0.03s
```

`-v` gives one line per test with its name, which is what you want when something fails.

| Command | Runs |
| --- | --- |
| `pytest` | every test found |
| `pytest test_calculator.py` | one file |
| `pytest test_calculator.py::test_add_two_positives` | one test |
| `pytest -v` | verbose, one line each |
| `pytest -q` | quiet |
| `pytest -x` | stop at the first failure |
| `pytest -k add` | only tests whose name contains "add" |

## Reading a Failure

This is what pytest is really for. Two deliberately broken tests:

```python
from calculator import add, average

def test_add_is_wrong():
    assert add(2, 3) == 6

def test_average_of_empty():
    assert average([]) == 0
```

**Output:**

```
============================= test session starts =============================
collected 2 items

test_failing.py FF                                                       [100%]

================================== FAILURES ===================================
______________________________ test_add_is_wrong ______________________________

    def test_add_is_wrong():
>       assert add(2, 3) == 6
E       assert 5 == 6
E        +  where 5 = add(2, 3)

test_failing.py:5: AssertionError
____________________________ test_average_of_empty ____________________________

    def test_average_of_empty():
>       assert average([]) == 0
               ^^^^^^^^^^^

test_failing.py:9:
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

numbers = []

    def average(numbers):
>       return sum(numbers) / len(numbers)
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^
E       ZeroDivisionError: division by zero

calculator.py:12: ZeroDivisionError
=========================== short test summary info ===========================
FAILED test_failing.py::test_add_is_wrong - assert 5 == 6
FAILED test_failing.py::test_average_of_empty - ZeroDivisionError: division b...
============================== 2 failed in 0.06s ==============================
```

`F` replaced the dot for each failure, and then pytest explained both.

The first is an **assertion failure**. `assert 5 == 6` and `where 5 = add(2, 3)` show the comparison *and* where the wrong value came from. A plain `AssertionError` would have said only that something was false; pytest rewrites assertions to report the actual values.

The second is a **different failure entirely**. The test did not fail its assertion — the code raised `ZeroDivisionError` before the assertion was reached. pytest followed the call into `calculator.py`, showed the failing line, and reported `numbers = []` as the argument that caused it.

That second failure is a real bug in `average()`, found by a test asking an obvious question. Deciding what to do about it — raise a clear error, or return `0` — is the subject of the next chapter.

Read failures bottom-up: the summary says which tests failed and why in one line each, and the detail above explains each one.

## Testing That Something Raises

Code that should reject bad input needs testing too, and a test that raises is normally a failing test. `pytest.raises` inverts that — it **passes** when the expected exception occurs:

```python
import pytest

from calculator import divide

def test_divide_by_zero_raises():
    with pytest.raises(ValueError):
        divide(1, 0)

def test_divide_by_zero_message():
    with pytest.raises(ValueError, match="cannot divide by zero"):
        divide(1, 0)
```

**Output:**

```
test_raises.py::test_divide_by_zero_raises PASSED                        [ 50%]
test_raises.py::test_divide_by_zero_message PASSED                       [100%]

============================== 2 passed in 0.02s ==============================
```

It is a context manager, so the code under test goes inside the `with` block. If no exception is raised, the test fails — which is right, because silently accepting bad input is the bug being guarded against.

`match` checks the message as well as the type, which is worth adding when the message is part of what you promise.

## Running One Test Many Times

Four cases of the same test means four near-identical functions. `parametrize` supplies the values instead:

```python
import pytest

from calculator import add

@pytest.mark.parametrize("a, b, expected", [
    (2, 3, 5),
    (-1, 1, 0),
    (0, 0, 0),
    (2.5, 0.5, 3.0),
])
def test_add(a, b, expected):
    assert add(a, b) == expected
```

**Output:**

```
test_param.py::test_add[2-3-5] PASSED                                    [ 25%]
test_param.py::test_add[-1-1-0] PASSED                                   [ 50%]
test_param.py::test_add[0-0-0] PASSED                                    [ 75%]
test_param.py::test_add[2.5-0.5-3.0] PASSED                              [100%]

============================== 4 passed in 0.02s ==============================
```

One function, four separate tests. Each is reported individually, so a failure names the exact values that broke — far better than one test that stops at the first bad case.

Adding a case is adding a line to the list.

## Fixtures

Tests that need the same starting data can share it through a **fixture** — a function decorated with `@pytest.fixture`, requested by naming it as a parameter:

```python
import pytest

@pytest.fixture
def students():
    return [
        {"name": "Anita", "mark": 78},
        {"name": "Ravi", "mark": 91},
    ]

def test_count(students):
    assert len(students) == 2

def test_top(students):
    assert max(students, key=lambda s: s["mark"])["name"] == "Ravi"
```

**Output:**

```
test_fixture.py::test_count PASSED                                       [ 50%]
test_fixture.py::test_top PASSED                                         [100%]

============================== 2 passed in 0.02s ==============================
```

Nothing called `students()`. pytest saw the parameter name, matched it to the fixture, and passed the result in.

The fixture runs **fresh for each test**, so one test cannot corrupt another's data. That isolation is the point: tests must pass in any order and on their own.

Fixtures shared across files go in a file called `conftest.py`, which pytest loads automatically.

## The Shape of a Test

A good test has three parts, conventionally called **Arrange, Act, Assert**:

```python
def test_average_of_three_marks():
    marks = [78, 91, 64]

    result = average(marks)

    assert result == pytest.approx(77.67, abs=0.01)
```

Set up the data, do the thing, check the result. Keeping them visually separate makes a test readable at a glance.

`pytest.approx` is the right way to compare floats, for the same reason `math.isclose` was — `77.666...` will not equal `77.67` exactly.

## What Makes a Test Worth Having

**One behaviour per test.** A test asserting five things stops at the first failure and hides the rest.

**A name that states the expectation.** `test_withdraw_fails_when_funds_are_short` tells you what broke from the summary line alone. `test_1` does not.

**No dependence on other tests.** Tests may run in any order, and in parallel. A test relying on one that ran before it will fail mysteriously.

**No dependence on the outside world.** A test that calls a real API fails when the network does, which trains people to ignore failures.

**Test the behaviour, not the implementation.** Assert what a function returns, not which internal variables it set. Tests tied to internals break on every refactor and stop being worth keeping.

Write the test when you write the code, or immediately after. Tests added months later cover what you remember rather than what matters.

## Further Reading

- **The pytest documentation** — https://docs.pytest.org/
- **Getting started with testing in Python** — https://realpython.com/python-testing/

A test is a function named `test_*` containing an `assert`, pytest finds and runs them, and its failure report shows the actual values and where they came from. Next, choosing the cases worth testing.
