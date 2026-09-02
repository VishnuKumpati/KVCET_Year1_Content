# Operators and Expressions

Programs often need to calculate something or check something. Python uses **operators** to do this.

```python
total = 80 + 90
```

Here `+` tells Python to add the two numbers. The numbers are the values being operated on, called **operands**, and the complete calculation `80 + 90` is an **expression**.

This chapter covers operators in the order you will use them: calculate, compare, then combine.

---

## Arithmetic Operators

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `+` | Add | `7 + 2` | `9` |
| `-` | Subtract | `7 - 2` | `5` |
| `*` | Multiply | `7 * 2` | `14` |
| `/` | Divide | `7 / 2` | `3.5` |
| `//` | Whole-number division | `7 // 2` | `3` |
| `%` | Remainder | `7 % 2` | `1` |
| `**` | Power | `7 ** 2` | `49` |

Four of these behave exactly as they do in school arithmetic. The other three need a word of explanation.

### Two Kinds of Division

`/` gives the exact division result and always produces a `float`. `//` gives the floor-division result, which is the whole-number result after rounding down.

```python
print(10 / 5)
print(10 // 5)
```

**Output**

```
2.0
2
```

Use `/` when you need the actual division result. Use `//` when you need complete units. Dividing ₹2470 among four people gives ₹617.50 with `/`, but `2470 // 4` is `617`, the whole rupees each person can actually be handed.

**Watch out.** Dividing by zero has no answer. `10 / 0` stops the program with `ZeroDivisionError`, and so do `//` and `%`.

### Remainder

`%` gives the remainder left after division.

```python
print(7 % 2)
```

**Output**

```
1
```

It is also useful for checking whether a number is divisible by another. `number % 2 == 0` means the number is even, and `number % 5 == 0` means it is a multiple of five.

`//` and `%` are often used together, one giving the whole units and the other the leftover.

### Power

`**` raises a number to a power.

```python
print(2 ** 3)
```

**Output**

```
8
```

---

## Operator Precedence

When an expression contains several operators, Python follows a fixed order called **operator precedence**.

```python
print(2 + 3 * 4)
```

**Output**

```
14
```

Python multiplied first, so the line means `2 + (3 * 4)`. To get a different order, use brackets:

```python
print((2 + 3) * 4)
```

**Output**

```
20
```

The full order, highest first:

| Order | Operators |
|---|---|
| 1 | `()` |
| 2 | `**` |
| 3 | `*` `/` `//` `%` |
| 4 | `+` `-` |
| 5 | `<` `<=` `>` `>=` `==` `!=` |
| 6 | `not` |
| 7 | `and` |
| 8 | `or` |

Rows 5 to 8 matter later in this chapter: all arithmetic is grouped before any comparison, and all comparisons are grouped before `and` or `or`.

When the intended order matters, use brackets. They make the calculation clear to both Python and the reader.

**Watch out.** A missing bracket produces no error. `88 + 76.5 + 91 / 3` is a legal line, so Python runs it and returns a wrong average with nothing to signal the fault.

---

## Comparison Operators

Comparison operators ask a question about two values. The answer is always `True` or `False`.

| Operator | Meaning | Example |
|---|---|---|
| `==` | equal to | `5 == 5` |
| `!=` | not equal to | `5 != 4` |
| `>` | greater than | `5 > 4` |
| `<` | less than | `5 < 4` |
| `>=` | greater than or equal | `5 >= 5` |
| `<=` | less than or equal | `5 <= 5` |

The one confusion worth fixing straight away is the difference between one equals sign and two:

```
=   → store a value
==  → compare two values
```

Writing `=` where a comparison belongs is a syntax error, and Python's message names `==` as the symbol you probably wanted.

Python also allows a range to be written the way it appears on paper, which most other languages do not:

```python
marks = 75
print(40 <= marks <= 100)
```

**Output**

```
True
```

**Watch out.** `0.1 + 0.2 == 0.3` is `False`. Decimal values cannot be stored exactly, so a small error is left behind in the sum and `==` notices it. Never test decimals for exact equality; use `math.isclose(a, b)` instead.

**Watch out.** `"Zoe" < "adam"` is `True`. Text is compared character by character in Unicode order, where every capital letter comes before every small letter, so this is not the alphabetical order you expect.

---

## Logical Operators

Sometimes one condition is not enough.

```python
marks = 82
attendance = 71

print(marks >= 40 and attendance >= 75)
```

**Output**

```
False
```

The marks are high, but the result is `False`, because `and` requires both conditions to be true.

| Operator | Meaning |
|---|---|
| `and` | both must be true |
| `or` | at least one must be true |
| `not` | reverses true and false |

```python
print(marks >= 40 or attendance >= 75)
print(not marks >= 40)
```

**Output**

```
True
False
```

`or` is `True` because the first condition holds. `not` turns the `True` from `marks >= 40` into `False`.

Because comparison outranks `and` and `or` in the precedence table, a line like `marks >= 40 and attendance >= 75` needs no brackets.

---

## Short-Circuit Evaluation

With `and`, Python stops as soon as it finds `False`. With `or`, Python stops as soon as it finds `True`. Whatever comes after that point is never worked out.

This is what makes a risky calculation safe:

```python
count = 0
total = 340

print(count != 0 and total / count > 50)
```

**Output**

```
False
```

The first condition is `False`, so Python does not evaluate the division, and the division-by-zero error never happens.

Swapping the two sides breaks it: the division would then run first and stop the program. Put the condition that protects the risky operation first.

---

## Truthy and Falsy Values

Python can treat any value as either truthy or falsy when a condition is expected.

| Value | Boolean meaning |
|---|---|
| `False` | falsy |
| `0`, `0.0` | falsy |
| `""` | falsy |
| `None` (no value at all) | falsy |
| Everything else | generally truthy |

```python
print(bool(0))
print(bool("0"))
```

**Output**

```
False
True
```

`0` is a number with no value, but `"0"` is a non-empty string. Python checks whether the string holds any characters, not what the text says. By the same rule, `bool("False")` is `True`, and so is `bool(" ")`, because a space is a character.

---

## Operators with Input

`input()` always gives you text. The operators then behave according to that type, not according to what you meant.

```python
first = input("First number: ")
second = input("Second number: ")
print(first + second)
```

**Output**

```
First number: 10
Second number: 20
1020
```

Python joined two strings instead of adding two numbers. Converting at the boundary fixes it:

```python
first = int(input("First number: "))
second = int(input("Second number: "))
print(first + second)
```

**Output**

```
First number: 10
Second number: 20
30
```

Convert input as soon as it enters the program, using `int` for counted quantities and `float` for measured ones.

**Watch out.** `"9" > "10"` is `True`, because Python compares the strings as text rather than as numbers. Convert numeric input to `int` or `float` before comparing it, or a ranking built on the comparison will be wrong with no error shown.

---

## Putting It Together

One short program uses arithmetic, comparison, logical operators, input conversion and formatting at once:

```python
maths = float(input("Mathematics: "))
physics = float(input("Physics: "))
attendance = float(input("Attendance (%): "))

average = (maths + physics) / 2
eligible = maths >= 40 and physics >= 40 and attendance >= 75

print(f"Average  : {average:.2f}")
print(f"Eligible : {eligible}")
```

**Output**

```
Mathematics: 88
Physics: 76.5
Attendance (%): 82
Average  : 82.25
Eligible : True
```

---

## Practice

Write each one in its own file inside `python-basics`, or one cell per program in Google Colab. Prompt for every value, convert it as it arrives, and use f-strings for all output.

**1. Seconds converter.** Read a whole number of seconds and report it as hours, minutes and seconds.

**Expected output**

```
Enter total seconds: 7385
2 hours, 3 minutes, 5 seconds
```

**Hint.** `//` and `%` are each needed twice. Hours come from dividing by 3600. What is left after the whole hours is `total % 3600`, and the minutes come out of that leftover.

**2. Splitting a bill.** Read a bill in whole rupees and a number of people. Report each share and the remainder that cannot be divided.

**Expected output**

```
Total bill: 2470
Number of people: 4
Each person pays: 617
Remaining: 2
```

**Hint.** Both values are counted quantities, so `int` is the converter. The two answers come from `//` and `%` on the same pair of numbers.

**3. Simple interest.** Read the principal, the yearly rate and the number of years. Interest is the principal multiplied by the rate and the time, divided by 100.

**Expected output**

```
Principal amount: 25000
Annual rate (%): 7.5
Time (years): 3
Interest        : 5625.00
Total repayable : 30625.00
```

**Hint.** All three are measured quantities, so read them with `float`. The two decimal places need `:.2f` in the f-string, because the value itself is `5625.0` and would otherwise print with one.

**4. Number report.** Read one whole number and print four Boolean answers: whether it is even, whether it is divisible by five, whether it lies between 100 and 999 inclusive, and whether it is below 0 or above 1000.

**Expected output**

```
Enter a number: 250
Even             : True
Divisible by 5   : True
Three digits     : True
Outside 0 to 1000: False
```

**Hint.** Each line is one expression that produces a `bool`, so nothing here needs an `if`. Evenness is a remainder test, the three-digit line is a chained comparison, and the last line is the one place `or` is needed.

**5. Eligibility.** Read three marks and an attendance percentage. A student is eligible when every mark is 40 or more and attendance is at least 75. Print the average to two decimal places and the eligibility as a single Boolean, written as one expression.

**Expected output**

```
Mathematics: 88
Physics: 76.5
Chemistry: 91
Attendance (%): 82
Average  : 85.17
Eligible : True
```

**Hint.** Four comparisons joined by three `and` operators, stored in one variable. No brackets are needed, because comparison outranks `and`. After it works, change the attendance to 60: the eligibility must turn `False` while the average stays the same.

---

## Readiness Check

- You can state what `/` gives that `//` does not.
- You have run a line where a missing bracket changed the answer, and confirmed that no error appeared.
- You can explain why `count != 0 and total / count > 50` is safe and the reversed version is not.
- You can explain why `"9" > "10"` is `True`.
- All five practice programs run and match the output shown.

---

## Reference

**[Operator Precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence)** — the complete precedence table from the Python Language Reference.

**[Numeric Types — int, float, complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)** — the exact definition of every arithmetic operator.

**[Truth Value Testing](https://docs.python.org/3/library/stdtypes.html#truth-value-testing)** — the authoritative list of falsy values.
