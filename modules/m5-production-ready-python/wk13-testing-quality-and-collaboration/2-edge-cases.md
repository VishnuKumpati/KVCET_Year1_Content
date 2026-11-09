# Edge Cases

A function can pass every test with ordinary inputs and still fail in real use. Many bugs stay hidden until the program receives an unusual input: a value exactly at a limit, an empty list, zero, or a value it should never accept. Users do enter such values, so a well-tested program checks them deliberately.

## What Edge Cases Are

> **An edge case is an unusual input or situation that may cause a program to behave unexpectedly, especially near the limits of what it is designed to handle.**

Inputs can be divided into two groups:

- A **typical case** is an ordinary input that the program handles most of the time, such as marks of `75` out of `100`.
- An **edge case** is an unusual input or situation that may cause unexpected behaviour, such as marks at the limits of the valid range or an empty list.

Edge cases deserve special attention because programmers usually think about typical inputs while writing code. Mistakes such as using `>` instead of `>=` only show up when the input is exactly at the limit.

## Common Types of Edge Cases

| Type | Meaning | Example |
| --- | --- | --- |
| **Boundary values** | Values exactly at a limit, or just next to it | `0`, `39`, `40` and `100` for marks from 0 to 100 with a pass mark of 40 |
| **Zero** | The value `0`, which often behaves differently from other numbers | Dividing by `0` |
| **Negative numbers** | Values below zero, when only positive values make sense | Marks of `-1` |
| **Empty values** | An empty string, list or dictionary | `""` or `[]` |
| **Very large values** | Values much larger than usual, but still valid | A very large number of items in a shopping cart |
| **Invalid input** | A value the function should reject | Marks of `101` |

## The Example Function

This topic tests a function that decides whether a student has passed.

**The rules:**

- Valid marks are from `0` to `100`, including both values.
- Marks of `40` or more mean **Pass**, and marks below `40` mean **Fail**.
- Marks outside `0` to `100` are invalid, so the function raises a `ValueError`.

Create a new folder named `results_project`, activate a virtual environment with pytest installed, as in **Testing with pytest**, and create this file:

**File: `results.py`**

```python
def get_result(marks):
    if marks < 0 or marks > 100:
        raise ValueError("Marks must be between 0 and 100")
    if marks >= 40:
        return "Pass"
    return "Fail"
```

- The first `if` rejects marks below `0` or above `100` by raising a `ValueError`.
- The second `if` returns `"Pass"` for marks of `40` or more.
- Any other valid marks return `"Fail"`.

## Boundary Value Analysis

The most important edge cases are the values at the **boundaries**.

> **A boundary value is a value at the edge of a valid range, where the program's behaviour changes, such as the lowest or highest allowed value, or the value where a result changes from one outcome to another.**

> **Boundary value analysis is a testing technique that checks the values at each boundary and the values just inside and just outside it.**

`get_result()` has three boundaries:

| Boundary | Values to Test | Expected Result |
| --- | --- | --- |
| **Lowest valid marks** | `-1` (just outside), `0` (at the boundary) and `1` (just inside) | `ValueError`, `"Fail"` and `"Fail"` |
| **Pass mark** | `39` (just below), `40` (at the boundary) and `41` (just above) | `"Fail"`, `"Pass"` and `"Pass"` |
| **Highest valid marks** | `99` (just inside), `100` (at the boundary) and `101` (just outside) | `"Pass"`, `"Pass"` and `ValueError` |

Testing the value below, at and above each boundary checks every place where the function's behaviour changes. The nine values include **seven valid** marks, which return `"Pass"` or `"Fail"`, and **two invalid** marks, `-1` and `101`, which raise a `ValueError`.

## Testing Boundary Values

Start with the pass mark, where the result changes from `"Fail"` to `"Pass"`.

**File: `test_results.py`**

```python
from results import get_result


def test_marks_just_below_pass_mark():
    assert get_result(39) == "Fail"


def test_marks_at_pass_mark():
    assert get_result(40) == "Pass"
```

Run `python -m pytest`, and both tests pass.

### Why This Test Matters

Suppose the pass condition were written as `marks > 40` instead of `marks >= 40`. A typical input, such as `75`, would still return `"Pass"`, so a test with `75` would not notice the mistake. But `get_result(40)` would now return `"Fail"`, and `test_marks_at_pass_mark` would fail:

**Possible output (shortened):**

```
>       assert get_result(40) == "Pass"
E       AssertionError: assert 'Fail' == 'Pass'
```

The boundary value `40` found a mistake that typical inputs could not.

## Testing Many Inputs with `parametrize`

Testing all nine boundary values with separate functions would mean writing nearly the same test again and again. pytest provides a shorter way.

> **Parametrisation is a technique for running one test function several times, each time with a different set of input and expected values. In pytest, it is done with the `@pytest.mark.parametrize` decorator.**

`@pytest.mark.parametrize` is a **decorator**, covered in **Decorators**. It is written directly above the test function.

**Syntax:**

```
@pytest.mark.parametrize("parameter_names", [list_of_values])
def test_name(parameter_names):
    assert ...
```

Replace the two tests in `test_results.py` with one parametrised test that checks all seven valid boundary values:

```python
import pytest
from results import get_result


@pytest.mark.parametrize("marks, expected", [
    (0, "Fail"),
    (1, "Fail"),
    (39, "Fail"),
    (40, "Pass"),
    (41, "Pass"),
    (99, "Pass"),
    (100, "Pass"),
])
def test_get_result_at_boundaries(marks, expected):
    assert get_result(marks) == expected
```

How it works:

1. `import pytest` is needed, because `@pytest.mark.parametrize` belongs to pytest.
2. `"marks, expected"` names the two variables that receive the values from each tuple.
3. The list contains seven **tuples**. Each tuple is one test case: the first value goes into `marks`, and the second goes into `expected`.
4. pytest runs `test_get_result_at_boundaries()` **seven times**, once for each tuple.
5. Each run is reported as a **separate test**, so if one value fails, the others still run.

Run `python -m pytest -v`:

**Possible output (shortened):**

```
test_results.py::test_get_result_at_boundaries[0-Fail] PASSED            [ 14%]
test_results.py::test_get_result_at_boundaries[1-Fail] PASSED            [ 28%]
test_results.py::test_get_result_at_boundaries[39-Fail] PASSED           [ 42%]
test_results.py::test_get_result_at_boundaries[40-Pass] PASSED           [ 57%]
test_results.py::test_get_result_at_boundaries[41-Pass] PASSED           [ 71%]
test_results.py::test_get_result_at_boundaries[99-Pass] PASSED           [ 85%]
test_results.py::test_get_result_at_boundaries[100-Pass] PASSED          [100%]
```

The values in square brackets, such as `[40-Pass]`, show which tuple each run used. If the `40` case failed, its name, `[40-Pass]`, would show exactly which input caused the failure.

## Testing That an Exception Is Raised

For marks of `-1` and `101`, the **expected result** is not a returned value. It is a `ValueError`. A normal `assert` cannot check this, because the error stops the function before it returns anything. pytest provides **`pytest.raises()`** for this.

> **`pytest.raises()` is used with a `with` statement to check that the code inside the block raises a specific exception. The test passes if the code inside the `with` block raises the expected exception. If it does not, pytest marks the test as failed.**

**Syntax:**

```
with pytest.raises(ExceptionType):
    code_that_should_raise_the_exception
```

Add this test to `test_results.py`:

```python
@pytest.mark.parametrize("marks", [-1, 101])
def test_get_result_rejects_marks_outside_range(marks):
    with pytest.raises(ValueError):
        get_result(marks)
```

- `@pytest.mark.parametrize("marks", [-1, 101])` runs the test twice. With only **one** parameter, each value is written on its own, without a tuple.
- `with pytest.raises(ValueError):` tells pytest that the code inside the block is **expected** to raise a `ValueError`.
- `get_result(marks)` raises a `ValueError` for both `-1` and `101`, so both runs **pass**.

### When the Expected Exception Is Not Raised

Suppose the check for marks above `100` were missing, so the first line of `get_result()` were only `if marks < 0:`. Then `get_result(101)` would return `"Pass"` instead of raising an error, and the test would fail:

**Possible output (shortened):**

```
_______________ test_get_result_rejects_marks_outside_range[101] _______________

marks = 101

    @pytest.mark.parametrize("marks", [-1, 101])
    def test_get_result_rejects_marks_outside_range(marks):
>       with pytest.raises(ValueError):
E       Failed: DID NOT RAISE ValueError
```

`DID NOT RAISE ValueError` means the code inside the `with` block finished **without** raising the expected exception. The `[101]` in the test name shows that the problem is the value `101`.

## Testing Empty Values

An empty list is another common edge case. Add a second function to `results.py`, which calculates the average of a list of marks:

```python
def average(numbers):
    if len(numbers) == 0:
        raise ValueError("Cannot calculate the average of an empty list")
    return sum(numbers) / len(numbers)
```

Without the `if` check, `average([])` would attempt to divide by zero and raise a `ZeroDivisionError`. The explicit check raises a `ValueError` with a message that explains why the operation cannot be performed.

Add these tests to `test_results.py`, and change the import line to `from results import get_result, average`:

```python
def test_average_of_numbers():
    assert average([80, 90, 70]) == 80


def test_average_of_empty_list_raises_error():
    with pytest.raises(ValueError, match="empty list"):
        average([])
```

- `test_average_of_numbers()` checks a **typical case**.
- `test_average_of_empty_list_raises_error()` checks the **edge case**: an empty list.
- The **`match`** argument checks that the error message contains the text `"empty list"`. This confirms that the **correct** error was raised, not a different `ValueError`.

## The Complete Test File

**File: `test_results.py`**

```python
import pytest
from results import get_result, average


@pytest.mark.parametrize("marks, expected", [
    (0, "Fail"),
    (1, "Fail"),
    (39, "Fail"),
    (40, "Pass"),
    (41, "Pass"),
    (99, "Pass"),
    (100, "Pass"),
])
def test_get_result_at_boundaries(marks, expected):
    assert get_result(marks) == expected


@pytest.mark.parametrize("marks", [-1, 101])
def test_get_result_rejects_marks_outside_range(marks):
    with pytest.raises(ValueError):
        get_result(marks)


def test_average_of_numbers():
    assert average([80, 90, 70]) == 80


def test_average_of_empty_list_raises_error():
    with pytest.raises(ValueError, match="empty list"):
        average([])
```

Run `python -m pytest`:

**Possible output (shortened):**

```
collected 11 items

test_results.py ...........                                              [100%]

============================== 11 passed in 0.02s ==============================
```

pytest counts **11 tests**: seven runs of the boundary test, two runs of the out-of-range test, and the two average tests.

## Finding Edge Cases for Any Function

Before writing tests for a function, work through these points:

1. **Find the limits.** Note the lowest and highest values the function accepts, and test the values at and just outside each limit.
2. **Find where the result changes.** Note every value where the output switches, such as a pass mark, and test the values on both sides.
3. **Try zero and negative numbers**, if the function works with numbers.
4. **Try empty values**, such as `""`, `[]` or `{}`, if the function works with strings or collections.
5. **Try invalid input**, and decide what the function should do: return a result, or raise an exception.

## Points to Remember

- **An edge case** is an unusual input or situation that may cause a program to behave unexpectedly, especially near the limits of what it is designed to handle.
- **Boundary value analysis** tests the values at each boundary and just inside and outside it.
- **`@pytest.mark.parametrize`** runs one test function several times with different values, reporting each run separately.
- **`pytest.raises()`** checks that code raises an expected exception. If it is not raised, the test fails with `DID NOT RAISE`.

## Further Reading

- 📎 **Official pytest guide to parametrisation** — https://docs.pytest.org/en/stable/how-to/parametrize.html
- 📎 **Official pytest guide to assertions about expected exceptions** — https://docs.pytest.org/en/stable/how-to/assert.html#assertions-about-expected-exceptions

Tests check whether code produces the correct results, but they do not show what a program is doing while it runs in real use. In the next topic, **Logging**, you'll learn how to record what happens inside a running program.
