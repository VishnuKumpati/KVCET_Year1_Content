# Variables, Data Types, and Input/Output

A program that prints one fixed line does the same thing every time it runs. This chapter covers what turns it into something worth running: holding a value under a name, knowing what kind of value it is, changing a value from one kind into another, presenting it clearly on the screen, and taking a value from the person using the program.

---

## Variables

A program needs to remember information while it runs: a name that was typed in, a mark, a total. A **variable** gives that information a name, so the program can use it again later.

### Storing a Value

Write the name, an equals sign, and the value:

```python
student_name = "Kavya"
marks = 88
```

Picture each line as a name pointing at a value:

```
student_name → "Kavya"
marks        → 88
```

From that point on, `student_name` stands for the text `Kavya` and `marks` for the number 88, anywhere in the program.

### The Four Assignment Rules

**The equals sign means "store this", not "is equal to".** Python works out the value on the right first, then attaches the name on the left to it. Read `marks = 88` as *marks takes the value 88*.

**A name can be given a new value at any time.** After the second line below, `marks` holds 91 and the earlier value is gone.

```python
marks = 88
marks = 91
```

**A name that was never given a value cannot be used.** Python does not treat it as empty or as zero. The program stops.

```python
print(score)
```

**Error**

```
NameError: name 'score' is not defined
```

Your screen will show several lines above this one. The last line is the one that names the problem.

**Assigning one variable to another copies the value at that moment.**

```python
a = 10
b = a
a = 25
print(b)
```

**Output**

```
10
```

Line 2 gave `b` the value 10. Line 3 attached `a` to a different value, which does not change the 10 that `b` is holding:

```
a → 25
b → 10
```

### Naming a Variable

The name of a variable is called an **identifier**. Three rules are enforced by Python itself, and breaking any of them stops the program.

| Rule | Accepted | Rejected |
|---|---|---|
| Letters, digits and underscore only | `total_marks` | `total-marks`, `total marks` |
| Cannot begin with a digit | `marks2` | `2marks` |
| Python's own keywords are reserved | `class_name` | `class` |

A fourth point is not rejected by Python but catches people just as often: capitals and small letters are different, so `Total` and `total` are two separate names.

```python
total = 100
print(Total)
```

**Error**

```
NameError: name 'Total' is not defined. Did you mean: 'total'?
```

Python suggests the name you meant, but the message still calls your name undefined. When a name you are certain exists is reported as undefined, check its capitals first.

There is nothing to memorise about keywords, since an editor colours them differently, but the full list is available at any time:

```python
import keyword
print(keyword.kwlist)
```

Three further points are **conventions**, not rules. Python will not object if you ignore them, but anyone reading your code will expect them.

| Convention | Good | Poor |
|---|---|---|
| Small letters, words joined by underscore, called **snake_case** | `student_name` | `StudentName`, `studentname` |
| The name states what it holds | `average` | `a`, `x1` |
| Never reuse a name Python already uses | `total` | `print`, `input`, `type` |

The last convention looks like the third rule above but behaves differently. `class` is a keyword, so Python refuses it outright. `print` is a built-in name, so Python accepts `print = 5` without complaint, and printing simply stops working for the rest of that program.

### Comments

A **comment** is a note written for people, which Python ignores completely. Anything after a `#` on a line is skipped, unless the `#` sits inside quotation marks, where it is part of the text.

```python
# Marks of three subjects
total = 240
average = total / 3      # three papers, so the average is the fair comparison
```

A comment should explain **why** a line exists, not repeat what it already shows:

| Comment | Verdict |
|---|---|
| `total = 240  # set total to 240` | Poor. The line already says this |
| `total = 240  # maximum across three 80-mark papers` | Good. Explains where 240 came from |

Python has no symbol for a multi-line comment, so start each line with `#`. Putting a `#` in front of a working line also switches that line off while testing, without deleting it.

---

## Data Types

### The Role of the Data Type

An **operator** is a symbol that combines values into a new value, such as `+`, `-`, `*` and `/`. What an operator does depends on the type of the values it is given, so the same symbol can mean two different things:

```python
print(5 + 3)
print("5" + "3")
```

**Output**

```
8
53
```

The first `+` added two numbers. The second joined two pieces of text, because for text, `+` means join. Chapter 3 covers the operators in full.

Every value belongs to a category, and the **data type** of a value is the category it belongs to. That is what Python read in order to choose between those two meanings of `+`.

### The Four Core Types

| Type | Holds | Example value |
|---|---|---|
| `int` | A whole number | `24`, `-5` |
| `float` | A number with a decimal point | `88.5`, `-0.75` |
| `str` | Text, inside quotation marks | `"Anitha"`, `'Chennai'` |
| `bool` | `True` or `False`, nothing else | `True` |

**`int`: whole numbers.** There is no limit on how large an integer can be.

```python
roll_number = 24
temperature = -5
```

**`float`: numbers with a decimal point.** Decimal numbers are stored using floating-point representation, so some decimal values cannot be represented exactly. That is why you may sometimes see a result like this:

```python
print(0.1 + 0.2)
```

**Output**

```
0.30000000000000004
```

To control how many decimal places you get, use `round`, which takes the value and the number of places you want:

```python
print(round(0.1 + 0.2, 2))
print(round(86.3333, 2))
```

**Output**

```
0.3
86.33
```

**`str`: text.** Single and double quotes work equally well, as long as you close what you opened. Digits inside quotes are text, not numbers, which is why `"25" + "30"` gives `"2530"` and not 55.

```python
name = "Anitha"
city = 'Chennai'
```

**`bool`: a yes-or-no answer.** Both values take a capital first letter. A Boolean answers a question such as *is this mark above forty*.

```python
is_present = True
```

### Checking the Type

The `type()` function reports the category of any value:

```python
print(type(24))
print(type(88.5))
print(type("Chennai"))
print(type(True))
```

**Output**

```
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

`class` is Python's general word for a kind of thing, so read `<class 'int'>` as *this is an integer*.

You never have to state in advance what type a variable will hold. Python is **dynamically typed**: the type belongs to the value, not to the name, so the same name can hold an integer on one line and a string on the next.

```python
value = 10
print(type(value))
value = "10"
print(type(value))
```

**Output**

```
<class 'int'>
<class 'str'>
```

`type()` is most useful as a debugging tool. When a program gives a wrong answer or refuses to run, printing the types involved is usually the fastest route to the cause.

---

## Converting Between Types

### Conversion Functions

Each is named after the type it produces.

| Function | Produces | Example | Result |
|---|---|---|---|
| `int()` | A whole number | `int("25")` | `25` |
| `float()` | A decimal number | `float("25.5")` | `25.5` |
| `str()` | Text | `str(25)` | `"25"` |
| `bool()` | `True` or `False` | `bool(0)` | `False` |

### Conversion Pitfalls

**`int()` on a decimal discards the fractional part instead of rounding.** Use `round()` when you want rounding.

```python
print(int(9.7))
print(round(9.7))
```

**Output**

```
9
10
```

**`int()` refuses text containing a decimal point.** `float()` accepts it, so when a value might carry a decimal point, convert with `float()`.

```python
print(float("25.5"))
```

**Output**

```
25.5
```

```python
print(int("25.5"))
```

**Error**

```
ValueError: invalid literal for int() with base 10: '25.5'
```

**Text that is not a number cannot be converted at all.** `int("twenty")` and `float("abc")` both stop the program with a `ValueError`. There is no partial conversion and no default of zero.

**`bool()` has one rule.** Everything is `True` except `0`, `0.0`, the empty string `""`, and empty collections, which you will meet in a later chapter.

```python
print(bool(0), bool(""), bool("False"))
```

**Output**

```
False False True
```

**Watch out.** `bool("False")` is `True`. `"False"` is a non-empty piece of text, and Python is not reading the word inside it.

### The One Conversion Python Does for You

**When an `int` and a `float` meet in one calculation, the `int` becomes a `float`**, because both values must be of the same type before the calculation can happen:

```python
print(5 + 2.5)
print(type(5 + 2.5))
```

**Output**

```
7.5
<class 'float'>
```

It never converts between text and numbers on its own. That is always your job:

```python
age = 20
print("Age: " + age)
```

**Error**

```
TypeError: can only concatenate str (not "int") to str
```

The fix is `str(age)`, which gives `+` two pieces of text to join.

---

## Producing Output

### Several Values at Once

Values separated by commas are displayed in order, with a single space between them. They may be of different types, and `print` converts each to text for you, so you do not have to call `str()` yourself.

```python
name = "Kavya"
marks = 88
print("Student:", name, "Marks:", marks)
```

**Output**

```
Student: Kavya Marks: 88
```

### Controlling the Separator and the Ending

`sep` changes what goes *between* the values; `end` changes what goes *after* the last one. By default `sep` is a single space and `end` is a newline, which is why the next `print` starts on a new line.

```python
print("2026", "09", "01", sep="-")
print("Loading", end="")
print("...done")
```

**Output**

```
2026-09-01
Loading...done
```

`print()` with nothing inside the round brackets prints a blank line, which is how output is spaced out.

### f-strings

Commas work, but they force a space between every value and give no control over how a number is displayed. An **f-string** is text with the letter `f` written before the opening quote. Inside it, anything you put in curly brackets is worked out, and the result is placed into the text.

```python
name = "Kavya"
marks = 88
print(f"{name} scored {marks} marks")
```

**Output**

```
Kavya scored 88 marks
```

The difference from commas is control over every character:

| Written as | Produces |
|---|---|
| `print("Total:", marks, "/100")` | `Total: 88 /100` |
| `print(f"Total: {marks}/100")` | `Total: 88/100` |

A calculation may go inside the curly brackets, not just a name:

```python
print(f"Half of the marks is {marks / 2}")
```

**Output**

```
Half of the marks is 44.0
```

Division always produces a `float` in Python, even when the answer has nothing after the decimal point, which is why 88 divided by 2 shows as `44.0` and not `44`.

**Watch out.** Forgetting the `f` is the most common f-string mistake. Without it Python has no reason to treat the curly brackets as special, so it prints them exactly as typed, and no error appears:

```python
print("{name} scored {marks}")
```

**Output**

```
{name} scored {marks}
```

### Formatting Numbers

Adding `:.2f` after the name inside the curly brackets displays a number to two decimal places. This is the normal way to present money, marks and measurements, and it saves calling `round()` before every line of output.

```python
percentage = 86.33333
print(f"Percentage: {percentage:.2f}")
print(percentage)
```

**Output**

```
Percentage: 86.33
86.33333
```

The formatting changes the display only; the variable itself is unchanged.

---

## Reading Input

### Asking a Question

The **`input`** function stops the program, waits for the person to type something and press Enter, and gives back what they typed.

```python
name = input("Enter your name: ")
print(f"Welcome, {name}")
```

**Output**

```
Enter your name: Kavya
Welcome, Kavya
```

In blocks like this one, `Kavya` on the first line is what you type. Everything else is what the program prints.

The text inside the round brackets is the **prompt**, and you should always write one. Without it the screen simply waits, and the person has no idea anything is expected.

### Input as Text

`input` gives back a string every time, even when the person types digits. This affects almost every program you write.

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

No error appeared. Python was asked to join two pieces of text and did exactly that, and a wrong answer with no error is harder to find than one that stops the program.

The digits typed do not make the value a number:

```python
marks = input("Marks: ")
print(type(marks))
```

**Output**

```
Marks: 88
<class 'str'>
```

### Converting at the Boundary

The **boundary** is the point where a value enters your program, which is the `input` line itself. Convert the value the moment it arrives, by wrapping the whole `input` inside `int` or `float`:

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

Read that line from the inside outwards: `input` collects the text, `int` converts it, and `=` stores the result.

| Choose | For quantities that are | Examples |
|---|---|---|
| `int` | Counted | An age, a number of people, a roll number |
| `float` | Measured | Marks, prices, temperature, distance |

When unsure, choose `float`, which accepts both `85` and `85.5`.

### Putting It Together

Everything above combines into one small program: read a student's details, convert each value at the boundary, and report them in a fixed layout.

```python
name = input("Student name: ")
roll_number = int(input("Roll number: "))
maths = float(input("Mathematics: "))
physics = float(input("Physics: "))
chemistry = float(input("Chemistry: "))

print()
print(f"{name} (Roll {roll_number})")
print(f"Mathematics : {maths:.1f}")
print(f"Physics     : {physics:.1f}")
print(f"Chemistry   : {chemistry:.1f}")
```

**Output**

```
Student name: Kavya Menon
Roll number: 24
Mathematics: 88
Physics: 76.5
Chemistry: 91

Kavya Menon (Roll 24)
Mathematics : 88.0
Physics     : 76.5
Chemistry   : 91.0
```

Ten lines, using a conversion chosen per value, `print()` for a blank line, and `:.1f` so that `88` and `76.5` line up as `88.0` and `76.5`.

---

## Recap

- Variables give values names.
- Data types tell Python what kind of value it is.
- Conversion functions change a value from one type into another.
- `input()` brings information into the program; `print()` sends it to the screen.

---

## Practice

Write each program in its own file inside `python-basics`. Prompt for every value, convert at the boundary, and use f-strings for all output. If you are working in Google Colab, write each program in its own cell instead, as described in the previous chapter.

**1. Student report card.** Extend the report program above so that it also reports the total and the average of the three marks, both to two decimal places.

**Example output**, using your own details:

```
Student name: Kavya Menon
Roll number: 24
Mathematics: 88
Physics: 76.5
Chemistry: 91

Kavya Menon (Roll 24)
Mathematics : 88.0
Physics     : 76.5
Chemistry   : 91.0
Total       : 255.50
Average     : 85.17
```

**2. Area and perimeter of a rectangle.** Ask for the length and the breadth; report both figures to two decimal places.

**Example output**

```
Enter length: 12.5
Enter breadth: 8
Area      : 100.00
Perimeter : 41.00
```

**3. Celsius into Fahrenheit.** Convert using nine fifths of the temperature plus thirty-two, displayed to two decimal places.

**Example output**

```
Temperature in Celsius: 36.6
36.6 C = 97.88 F
```

**4. Shop bill line.** Ask for an item name, a quantity that is counted, and a unit price that is measured. Report the line total. Decide deliberately which conversion each number needs.

**Example output**

```
Item: Notebook
Quantity: 3
Unit price: 45.50
3 x Notebook @ 45.50 = 136.50
```

**5. Type inspector.** Ask for one value, then print it four times: as it arrived with its type, as a float with its type, as an integer with its type, and converted back to text with its type. Run it with `7`, then `7.9`, then `hello`, and record in a comment what happened each time and why.

For the integer step, remember that `int()` refuses text containing a decimal point, so `7.9` must be converted through `float` first.

**Example output**

```
Enter a value: 7.9
As entered : 7.9 <class 'str'>
As float   : 7.9 <class 'float'>
As integer : 7 <class 'int'>
As text    : 7.9 <class 'str'>
```

---

## Reference

**[Built-in Types](https://docs.python.org/3/library/stdtypes.html)** — the authoritative description of `int`, `float`, `str` and `bool`.

**[Built-in Functions](https://docs.python.org/3/library/functions.html)** — the exact definition of every function used here: `print`, `input`, `type`, `int`, `float`, `str` and `round`.

**[Fancier Output Formatting](https://docs.python.org/3/tutorial/inputoutput.html)** — f-strings and the format specifications that follow the colon, including `:.2f`.
