# Operators and Expressions

You have three marks in front of you — 88 in Mathematics, 76.5 in Physics, 91 in Chemistry — and you want the average. Nobody needs a program to add three numbers, so try it anyway and watch what Python hands back:

```python
average = (88 + 76.5 + 91) / 3
print(average)
```

**Output**

```
85.16666666666667
```

The answer is right, and it also looks nothing like a mark you would write on a slip. That long tail of sixes is the first sign that Python's arithmetic has rules of its own, and this file is about those rules. Learn them here, in a five-line program, and you will not be hunting them later in a fifty-line one.

Two words will keep coming back, so take thirty seconds on them now. **An operator is a symbol that combines values into a new value** — in that line, `+` and `/`. The values it works on are its **operands**. **An expression is any piece of code that produces a value**, so `(88 + 76.5 + 91) / 3` is an expression, and so is plain `88`.

Keep a file open beside this page. Every section asks you to run something, and reading about an operator is not the same as watching it surprise you.

---

## Arithmetic Operators

Seven operators do arithmetic. Three of them you already know from school and they hold no surprises; the other four each have a habit that catches people, which is why most of this section is about those.

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `+` | Addition | `7 + 2` | `9` |
| `-` | Subtraction | `7 - 2` | `5` |
| `*` | Multiplication | `7 * 2` | `14` |
| `/` | True division | `7 / 2` | `3.5` |
| `//` | Floor division | `7 // 2` | `3` |
| `%` | Remainder | `7 % 2` | `1` |
| `**` | Power | `7 ** 2` | `49` |

### Two Kinds of Division

Python divides in two ways, and the day you meet the difference is usually the day you are dividing money.

Four friends split a hostel mess bill of ₹2470. Ask `/` for the answer and it gives 617.5, which is honest and useless — nobody hands over half a rupee. What you actually want is the whole rupees each person pays, and then whatever is left over.

`/` always produces a decimal number, even when the division comes out exact:

```python
print(10 / 5)
```

**Output**

```
2.0
```

`//` divides and rounds the result **down**, so you get whole units:

```python
print(2470 // 4)
```

**Output**

```
617
```

Ask for `/` when you want the exact value, and `//` when you want whole things — complete minutes, complete boxes, rupees a person can actually be handed.

**Try this now.** You have a ₹137 balance and you want to know how many ₹20 notes that is. Print `137 // 20`. You should see `6` — six notes, and something left over that the next section knows how to find.

### Remainder

`%` gives you exactly that leftover: what remains after floor division. `7 % 2` is `1`, because 2 goes into 7 three times and 1 is left standing.

`//` and `%` are two halves of one question, so you will almost always see them together. Split 7385 seconds into minutes and seconds and both appear in the same line:

```python
total_seconds = 7385
print(total_seconds // 60, "minutes and", total_seconds % 60, "seconds")
```

**Output**

```
123 minutes and 5 seconds
```

`%` has a second job that looks unrelated until you see it once. `n % 2 == 0` asks whether a number is even, because an even number leaves nothing behind when you divide it by two. The same trick with `% 5` finds multiples of five.

**Try this now.** Finish the ₹137 question: print `137 % 20`. The answer is `17`, and six twenties plus seventeen is exactly 137. Whenever a leftover has to go somewhere — the last installment, the last box, the last minute — this is the operator that finds it.

### Power

`**` raises a number to a power, and it will take a fractional exponent, so `0.5` gets you a square root without needing anything imported:

```python
print(2 ** 10)
print(9 ** 0.5)
```

**Output**

```
1024
3.0
```

### When You Divide by Zero

Every other operator gives you an answer. This one refuses, and `/`, `//` and `%` all refuse alike:

```python
print(10 / 0)
```

**Output**

```
ZeroDivisionError: division by zero
```

Read that message as help rather than as a complaint. Python has told you the operation has no answer and pointed at the line where you asked for it, which is far kinder than quietly printing a zero and letting a wrong number reach somebody's mark sheet. This error is not rare either — it turns up the first time you calculate an attendance percentage for a student who joined before any class was held. There is a way to protect that calculation, and it appears further down this page.

---

## Precedence and Parentheses

So far every example has held a single operator, and a single operator has nothing to argue with. Put two on one line and a question appears that you have been answering by instinct since school: which one goes first? Python answers it the same way every time, and the answer is worth knowing exactly, because the average at the top of this page depends on it.

**Precedence is the order in which operators are applied when one line contains more than one.** Python works down this order:

| Order | Operators | Group |
|---|---|---|
| 1 | `()` | Parentheses |
| 2 | `**` | Power |
| 3 | `*` `/` `//` `%` | Multiplication and division |
| 4 | `+` `-` | Addition and subtraction |
| 5 | `<` `<=` `>` `>=` `==` `!=` | Comparison |
| 6 | `not` `and` `or` | Logic |

Two consequences are worth carrying: all arithmetic finishes before any comparison happens, and all comparisons finish before `and` or `or` looks at the results. That second one saves you brackets, as you will see in a moment.

### Working an Expression Out Step by Step

When a line has several operators and you are not sure what it does, do what Python does — one operator at a time, highest first. Take `average >= 40 and attendance >= 75`, with `average` as `85.17` and `attendance` as `82`:

| Step | Expression | What was applied |
|---|---|---|
| 1 | `85.17 >= 40 and 82 >= 75` | Starting point |
| 2 | `True and 82 >= 75` | The first comparison |
| 3 | `True and True` | The second comparison |
| 4 | `True` | `and`, last of all |

Both comparisons finished before `and` saw anything, so that line needs no brackets at all. Get into the habit of reducing a line this way on paper; it is the same method that will untangle much longer conditions later.

### Where Brackets Change the Answer

Now look again at the average from the top of this page, with the brackets taken off:

```python
print(88 + 76.5 + 91 / 3)
print((88 + 76.5 + 91) / 3)
```

**Output**

```
194.83333333333334
85.16666666666667
```

Only the chemistry mark got divided, because `/` acts before `+`. Notice what Python did **not** do: it did not warn you, underline anything, or slow down. It ran a perfectly legal line and gave you a wrong average, and a wrong average is the kind of mistake that reaches a printed result sheet.

So make this a rule rather than a judgement call: **write the brackets wherever a reader would have to stop and work out the order.** They cost nothing, and the reader you are protecting is usually yourself, two weeks later.

### One Exception to Left-to-Right

Equal-precedence operators run left to right, so `10 - 2 - 3` is `(10 - 2) - 3`, which is `5`. There is one exception, and it is `**`.

**Predict before you run it.** Is `2 ** 3 ** 2` equal to 64 or 512?

<details>
<summary>Show the answer</summary>

**Output**

```
512
```

`**` groups right to left, so the line means `2 ** (3 ** 2)`, which is `2 ** 9`. Left to right it would have been `8 ** 2`, or 64. If you predicted 64, you were applying the rule that every other operator follows — which is exactly why this one is worth remembering.

</details>

---

## Comparison Operators

Rows 5 and 6 of that precedence table have been sitting there unexplained. They are where the arithmetic stops being arithmetic and starts being a decision.

Every decision a program makes comes down to a question about two values: *is this mark high enough, is the fee cleared, is this the right password.* A comparison operator asks one such question and answers `True` or `False` — nothing else.

| Operator | Asks | Example | Result |
|---|---|---|---|
| `==` | Are they equal? | `5 == 5` | `True` |
| `!=` | Are they different? | `5 != 4` | `True` |
| `>` | Is the left one bigger? | `5 > 9` | `False` |
| `<` | Is the left one smaller? | `5 < 9` | `True` |
| `>=` | Bigger, or the same? | `5 >= 5` | `True` |
| `<=` | Smaller, or the same? | `9 <= 5` | `False` |

One of these will bite you in your first week, and it is `==`. In everyday writing, `=` means *is equal to*. In Python, `=` stores a value and `==` asks a question, and every language that uses both symbols produces the same confusion in beginners. The good news is that Python catches it straight away instead of misbehaving quietly:

**Output**

```
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

When you see that line, you have written `=` where a question belonged. Python has even guessed what you meant.

### Chaining

Python lets you write a range the way you would on paper, which most other languages refuse:

```python
marks = 75
print(40 <= marks <= 100)
```

**Output**

```
True
```

Python reads that as `40 <= marks and marks <= 100`. Use it — `40 <= marks <= 100` is far easier to read six months from now than the long form.

### Comparing Decimals

This one surprises everybody the first time, so meet it here rather than in a lab test.

**Predict before you run it.** What does `print(0.1 + 0.2 == 0.3)` show?

<details>
<summary>Show the answer</summary>

**Output**

```
False
```

Add the two and print the sum on its own and you can see why:

```python
print(0.1 + 0.2)
```

**Output**

```
0.30000000000000004
```

Certain decimal fractions cannot be stored exactly in the binary a machine uses, in the same way that one-third cannot be written exactly as a decimal. A tiny error is left behind, and `==` notices it.

Nothing is broken, and this is not a Python quirk — every language storing decimals this way behaves identically. What it means for you is a habit: **never test decimals for exact equality.** Compare them with a tolerance instead:

```python
import math
print(math.isclose(0.1 + 0.2, 0.3))
```

**Output**

```
True
```

</details>

### Comparing Text

Comparison works on text too, and the ordering is not quite the alphabetical order you expect. Strings are compared character by character in Unicode order, and in that ordering every capital letter comes before every small letter:

```python
print("apple" < "banana")
print("Zoe" < "adam")
```

**Output**

```
True
True
```

`"apple" < "banana"` matches your intuition. `"Zoe" < "adam"` does not, and it is the honest answer: `Z` ranks before `a`. Any time you sort or compare names that people have typed themselves, this is waiting for you.

---

## Logical Operators

One comparison answers one question. A real rule usually needs two or three answers put together — *the marks are high enough **and** the attendance is high enough* is how your own college decides who sits the examination. Three operators do that joining.

| Operator | Result is `True` when |
|---|---|
| `and` | Both sides are `True` |
| `or` | At least one side is `True` |
| `not` | The single operand is `False` |

```python
marks = 82
attendance = 71

print(marks >= 40 and attendance >= 75)
print(marks >= 40 or attendance >= 75)
print(not marks >= 40)
```

**Output**

```
False
True
False
```

Look at the first line before moving on. The marks are comfortable, yet the answer is `False`, because `and` will not settle for one side out of two. That single line is the whole 75% attendance rule, and a student with 82 marks and 71% attendance is exactly who it is written for.

**Try this now.** Predict, then run: `print(45 >= 40 and 62 >= 75)`. It prints `False`. Change 62 to 80 and it prints `True`. That one line is an eligibility check, and nothing beyond this page was needed to write it.

### Stopping Early

`and` stops at the first `False`; `or` stops at the first `True`. Whatever comes after that point is never worked out at all — and that behaviour is a tool, not a technicality.

Remember the attendance percentage that divides by the number of classes held, and the student who joined before any class happened. Here is how you protect it:

```python
count = 0
total = 340
print(count != 0 and total / count > 50)
```

**Output**

```
False
```

`count != 0` is `False`, so `and` already knows the answer and the division on the right never runs. No crash, and the right answer.

Now swap the two sides around and run it. The division goes first, there is nothing standing in front of it, and the program stops with `ZeroDivisionError`. Same operator, same values, opposite outcome — which is why **the order of operands around `and` is part of the logic, not a matter of taste.** Write the test that protects first.

---

## Truthiness

`and`, `or` and `not` were described above as though they only ever see `True` and `False`. In practice they are handed marks, names, empty boxes and zeroes, and they cope with all of it — because Python has an answer for what every value counts as.

Any value at all can be used where `True` or `False` is expected. Values that count as `False` are called **falsy**; everything else is **truthy**. The falsy list is short enough to memorise, and there is nothing else on it:

| Falsy values | |
|---|---|
| `False` | The Boolean itself |
| `None` | The absence of a value |
| `0`, `0.0` | Zero, in either numeric type |
| `""` | The empty string |
| `[]`, `()`, `{}` | Empty collections |

`bool()` tells you which side any value falls on.

**Predict before you run it.** Which of `"0"`, `"False"` and `" "` — a string holding a single space — count as `True`?

<details>
<summary>Show the answer</summary>

```python
print(bool(0), bool(""), bool([]))
print(bool(-1), bool("0"), bool("False"), bool(" "))
```

**Output**

```
False False False
True True True True
```

All three are truthy, and this is the trap. Python does not read the words inside a string; it only asks whether the string has any characters in it. `"0"` has one character, `"False"` has five, and a space is a character too, so `" "` is not empty.

The number `0` is falsy. The text `"0"` is not. If you ever check whether a form field was filled in, that difference decides whether your check works.

</details>

---

## Type Conversion on Real Input

Everything above worked because the numbers were typed straight into the program. The moment values come from `input`, one fact changes everything: **`input` always hands you text, never a number** — and the operators react to that in two very different ways.

**Some operators say nothing.** `+` joins two strings and `*` repeats one, so both give you an answer. The wrong answer:

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

Ten plus twenty is not one thousand and twenty. Python did not add anything; it stuck `"10"` and `"20"` together, and it had no reason to think you wanted otherwise.

**Some operators stop the program.** `-`, `/`, `//`, `%` and `**` have no meaning for text at all:

```python
print(first - second)
```

**Output**

```
TypeError: unsupported operand type(s) for -: 'str' and 'str'
```

Of the two failures, this loud one is the lucky one. It names the problem, points at the line, and costs you a minute. The silent one from a moment ago costs nothing to run and everything to trust.

**And comparison never complains at all:**

```python
print("9" > "10")
```

**Output**

```
True
```

Character by character, `"9"` beats `"1"`, and the comparison stops there satisfied. As numbers, `9 > 10` is `False`. Picture that inside a program that ranks a class by marks: the ranking is wrong, every student sees it, and not one error message was printed.

The cure is a single habit, and it is worth more than any rule on this page. **Convert at the boundary** — the moment a value arrives, not later where you use it:

```python
maths = float(input("Mathematics: "))
```

Which converter? Use `int` for things you **count** — students, classes, rupees, notes. Use `float` for things you **measure** — marks, percentages, prices, temperature. When you cannot decide, choose `float`, because it accepts both `85` and `85.5` while `int` refuses the second.

Python will convert on its own, but only between numbers: an `int` quietly becomes a `float` when the two meet in one calculation, so `5 + 2.5` gives `7.5`. Between text and numbers it converts nothing, ever. That part is your job, on the line where the value comes in.

---

## Practice

Write each one in its own file. Prompt for every value, convert it as it arrives, and check your output against what is shown — matching it exactly is how you know you are done. Each problem has a hint you can open if you are stuck for more than a few minutes; the hint points, it does not solve.

**1. Seconds converter.** Read a whole number of seconds and report it as hours, minutes and seconds.

**Expected output**

```
Enter total seconds: 7385
2 hours, 3 minutes, 5 seconds
```

<details>
<summary>Hint</summary>

`//` and `%` are each needed twice. Hours come from dividing by 3600. What is left after the whole hours is `total % 3600`, and the minutes are hiding inside that leftover.

</details>

**2. Splitting a bill.** Read a bill in whole rupees and a number of people. Report each share and the amount that cannot be divided evenly.

**Expected output**

```
Total bill: 2470
Number of people: 4
Each person pays: 617
Remaining: 2
```

<details>
<summary>Hint</summary>

This is the mess-bill question from earlier in this page, with both values read from the keyboard. Both are counted quantities, so `int` is the converter.

</details>

**3. Simple interest.** Read the principal, the yearly rate and the number of years. Interest is the principal multiplied by the rate and the time, divided by 100.

**Expected output**

```
Principal amount: 25000
Annual rate (%): 7.5
Time (years): 3
Interest        : 5625.00
Total repayable : 30625.00
```

<details>
<summary>Hint</summary>

All three are measured quantities, so read them with `float`. `*` and `/` share a precedence level and run left to right, so `p * r * t / 100` is correct — brackets around the multiplication still make your intention clearer to a reader.

</details>

**4. Number report.** Read one whole number and print four Boolean answers: whether it is even, whether it is divisible by five, whether it lies between 100 and 999 inclusive, and whether it is either negative or above a thousand. Print the comparisons themselves.

**Expected output**

```
Enter a number: 250
Even             : True
Divisible by 5   : True
Three digits     : True
Outside 0 to 1000: False
```

<details>
<summary>Hint</summary>

Each line is one expression that produces a `bool`, so nothing here needs an `if`. Evenness is a remainder test, the three-digit line is a chained comparison, and the last line is the one place you need `or`.

</details>

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

<details>
<summary>Hint</summary>

Four comparisons joined by three `and` operators, stored in one variable. No brackets are needed, because comparison outranks `and` — the trace table earlier on this page is the proof. For the two decimal places, `round(average, 2)` will do it.

Then test it properly: change the attendance to 60 and the eligibility must turn `False` while the average stays the same.

</details>

---

## Before You Move On

Work through this list honestly — it is the difference between having read this page and being able to use it.

- You can say what `/` gives that `//` does not, without looking it up.
- You have run something where a missing bracket changed the answer, and you saw that no error appeared.
- You can explain why `count != 0 and total / count > 50` is safe and the reversed version is not.
- You know why `"9" > "10"` is `True`, and where in a program that should have been prevented.
- All five practice programs run and match the output shown.

Anything you cannot do yet points at one section above, and re-reading that section costs ten minutes now against an hour in a lab.

---

## Reference

**[Operator Precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence)** — the complete precedence table from the Python Language Reference.

**[Numeric Types — int, float, complex](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)** — the exact definition of every arithmetic operator.

**[Truth Value Testing](https://docs.python.org/3/library/stdtypes.html#truth-value-testing)** — the authoritative list of falsy values.
