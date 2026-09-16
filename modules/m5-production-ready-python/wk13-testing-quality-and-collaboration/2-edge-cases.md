# Edge Cases

A test that passes proves nothing on its own. `add(2, 3) == 5` would still pass if `add` were written to return `5` always.

Tests are only worth having if they would **fail** when the code is wrong. That means choosing inputs likely to expose a mistake, and those are almost never the ordinary ones.

An **edge case** is an input at the limit of what the code handles: the empty collection, the first and last item, zero, the exact boundary of a condition. Bugs cluster there, because the ordinary cases are what the author was thinking about while writing.

## The Categories

Run through this list for any function and most of its bugs will surface.

**Nothing.** An empty list, an empty string, an empty dictionary, `None`. What does `average([])` do?

**One.** A single item. Code written for "many" often assumes at least two — anything involving pairs, gaps between items, or "all but the last".

**Many.** Enough to expose an algorithm that only works for small inputs, and to notice one that is unusably slow.

**Boundaries.** The exact value a condition tests, and the values either side. If a rule says "85 and above is an A", then `84`, `85` and `86` are the cases that matter.

**Zero.** As an input, a divisor, a count, a length. Division by zero is the classic, and zero is falsy, which breaks `if value:` when `0` is legitimate data.

**Negatives.** Where only positives were expected. A negative quantity, age or price usually means something upstream went wrong.

**Duplicates.** Where uniqueness was assumed. Two students with the same name, two identical marks when ranking.

**Wrong types.** A string where a number was expected — usually from user input or a file, where everything is text.

**Extremes.** Very large numbers, very long strings, a file of a million rows.

**Awkward text.** Accents, emoji, right-to-left scripts, a name with an apostrophe. `len()` on text is not always the number of visible characters.

## Boundaries Are the Richest Source

Take a grading function:

```python
def grade(mark):
    if mark >= 85:
        return "A"
    elif mark >= 70:
        return "B"
    elif mark >= 50:
        return "C"
    return "F"
```

Testing `78` and `91` tells you almost nothing. Every boundary is a place where `>=` might have been `>`, or a threshold mistyped.

Three boundaries, two values each:

```python
import pytest

from grading import grade

@pytest.mark.parametrize("mark, expected", [
    (49, "F"),
    (50, "C"),
    (69, "C"),
    (70, "B"),
    (84, "B"),
    (85, "A"),
])
def test_grade_boundaries(mark, expected):
    assert grade(mark) == expected
```

**Output:**

```
test_grading.py::test_grade_boundaries[49-F] PASSED                      [  7%]
test_grading.py::test_grade_boundaries[50-C] PASSED                      [ 14%]
test_grading.py::test_grade_boundaries[69-C] PASSED                      [ 21%]
test_grading.py::test_grade_boundaries[70-B] PASSED                      [ 28%]
test_grading.py::test_grade_boundaries[84-B] PASSED                      [ 35%]
test_grading.py::test_grade_boundaries[85-A] PASSED                      [ 42%]
```

Six cases, one function. Change any `>=` to `>` and exactly one fails, naming the value that exposed it.

The technique has a name: **boundary value analysis** — for every threshold, test the value on it and the value below it.

Its companion is **equivalence partitioning**: divide the inputs into groups treated identically, and test one from each. For `grade`, the groups are the four bands, so `0`, `60`, `75` and `95` cover them. Testing `60`, `61` and `62` adds nothing, because they are the same case three times.

Together they give the rule: **one value from each group, plus both values at every boundary.**

## Off-by-One

The commonest bug this catches is the off-by-one, and it usually passes the obvious test:

```python
def last_three(items):
    return items[len(items) - 3:]
```

```python
def test_last_three_of_five():
    assert last_three([1, 2, 3, 4, 5]) == [3, 4, 5]

def test_last_three_of_two():
    assert last_three([1, 2]) == [1, 2]
```

**Output:**

```
test_buggy.py::test_last_three_of_five PASSED                            [ 50%]
test_buggy.py::test_last_three_of_two FAILED                             [100%]

================================== FAILURES ===================================
___________________________ test_last_three_of_two ____________________________

    def test_last_three_of_two():
>       assert last_three([1, 2]) == [1, 2]
E       assert [2] == [1, 2]
E
E         At index 0 diff: 2 != 1
E         Right contains one more item: 2
E
E         Full diff:
E           [
E         -     1,
E               2,
E           ]

test_buggy.py:9: AssertionError
```

The normal case passed. With fewer than three items, `len(items) - 3` is **negative**, and a negative index counts from the end — so `[1, 2][-1:]` returned `[2]`.

The fix is `items[-3:]`, which handles short lists correctly. The point is that no amount of staring at the normal case would have found this, and one edge case did immediately.

pytest's diff makes it obvious what went wrong: the expected list contains one more item than the actual, and the missing one is at the front.

## Deciding What Should Happen

Edge cases often have no obvious right answer, and that is the real value of looking for them: they force a decision that would otherwise be made by accident.

```python
def average(marks):
    return sum(marks) / len(marks)
```

What should `average([])` do? Three defensible answers:

**Return `0.0`.** Convenient for a report. Misleading — it implies an average of zero rather than no data.

**Raise `ValueError`.** Honest; an average of nothing is undefined. It puts the decision on the caller.

**Return `None`.** Explicitly "no answer", and every caller must handle it.

There is no universally correct choice — it depends on what the caller should do about it. What is not acceptable is the original, which raises `ZeroDivisionError`: an error about division, from a function the caller asked for an average, saying nothing about the real problem.

Decide, implement, and write the test that records the decision:

```python
def average(marks):
    if not marks:
        return 0.0
    return sum(marks) / len(marks)
```

```python
def test_average_of_empty_is_zero():
    assert average([]) == 0.0

def test_average_of_one():
    assert average([78]) == 78.0

def test_average_of_many():
    assert average([78, 91, 64]) == pytest.approx(77.67, abs=0.01)
```

**Output:**

```
test_grading.py::test_average_of_empty_is_zero PASSED                    [ 64%]
test_grading.py::test_average_of_one PASSED                              [ 71%]
test_grading.py::test_average_of_many PASSED                             [ 78%]
```

The empty case is now documented behaviour rather than an accident. Anyone changing it breaks a test that states the intent.

## Asking More of an Input

Another example, with several edges at once:

```python
def top_n(marks, n):
    return sorted(marks, reverse=True)[:n]
```

Three questions the ordinary case does not ask: what if there are fewer than `n` marks, what if there are none, and what if `n` is zero?

```python
def test_top_n_fewer_than_asked():
    assert top_n([78, 91], 5) == [91, 78]

def test_top_n_of_empty():
    assert top_n([], 3) == []

def test_top_n_zero():
    assert top_n([78, 91], 0) == []
```

**Output:**

```
test_grading.py::test_top_n_fewer_than_asked PASSED                      [ 85%]
test_grading.py::test_top_n_of_empty PASSED                              [ 92%]
test_grading.py::test_top_n_zero PASSED                                  [100%]
```

All three pass, because slicing already handles short lists gracefully. That is worth knowing rather than assuming — and the tests now guarantee it stays true if the implementation is ever rewritten with a loop.

## Finding Edge Cases

Four habits, roughly in order of usefulness.

**Read the function's own conditions.** Every `if`, every comparison, every `range` is a boundary with two test values.

**Ask what the input could be that you did not picture.** You imagined a list of marks. Could it be empty? Have one item? Contain a negative? Contain the same mark twice?

**Look at the types.** A parameter named `count` might receive `0`, or `-1`, or `"3"`. A string parameter might receive `""`.

**Write the test when you fix a bug.** Every bug found in use is an edge case you missed. A test written at that moment ensures it is found automatically if it ever returns — this is called a **regression test**, and it is the highest-value test there is, because you have proof the case occurs.

## How Much Is Enough

Exhaustive testing is impossible, and chasing a coverage percentage produces tests written to touch lines rather than to find bugs.

A reasonable standard for each function:

- one ordinary case, to show it works
- both values at every boundary
- the empty and single-item cases, where a collection is involved
- the error cases, checked with `pytest.raises`
- a regression test for every bug ever found in it

That is typically five to ten tests for a function with real logic, and one or two for a trivial one. It is far more valuable than fifty tests of ordinary values.

## Further Reading

- **The pytest documentation** — https://docs.pytest.org/
- **Boundary value analysis and equivalence partitioning** — https://www.guru99.com/equivalence-partitioning-boundary-value-analysis.html

Bugs live at the limits — empty, one, zero, negative, and either side of every threshold — and looking for them forces decisions about behaviour that would otherwise be made by accident. Next, the standards that make code readable in the first place.
