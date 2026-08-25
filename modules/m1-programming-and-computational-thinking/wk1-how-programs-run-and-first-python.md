# Foundations of Python Programming

## The Need for a Programming Language

Ask a friend to add three marks and tell you the average, and they will do it immediately. They know what marks are, they know what adding means, and they know how an average is worked out. A computer knows none of this. It is a machine that works only with numbers, and English words carry no meaning for it at all.

So how does anyone get useful work out of a machine like that? By writing the instructions in a language it can be made to understand. That is exactly what a programming language is — a small set of words and symbols, each with one fixed meaning, along with a translator that turns them into the numbers the machine finally works with.

Python is one of those languages, and it stays unusually close to English. You can almost read this line aloud and guess what it does:

```python
print("Welcome to Python")
```

It prints the words `Welcome to Python` on the screen, and that is all it does. Nothing has to be written around it to make it work, and that plainness is the main reason Python is chosen as a first language more often than any other.

## Compilers and Interpreters

Since every programming language has to be translated, the real question is not whether translation happens but when it happens. There are two answers, and they divide programming languages into two families.

A **compiler** finishes the translation in advance. It goes through your entire program, converts all of it, and hands you a new file that the computer can run on its own. It works the way a translated book does: someone translates the whole thing first, and only then can anyone read it. The reading is quick, but nothing can begin until the translation is complete. C and C++ belong to this family.

An **interpreter** translates while the program is running. It picks up one piece, carries out the work, and moves on to the next. It works the way a translator does at a meeting, turning each sentence into your language as it is spoken. The meeting can start at once, but the translator has to sit through the whole of it. Python belongs to this family.

## Python as an Interpreted Language

Because Python is interpreted, three things follow, and all three will shape the way you work.

- **You can run a program the moment you save it.** Nothing stands between writing a line and seeing what it does, which is the best possible situation for someone learning.
- **Python has to be present on the computer that runs your file.** Your file is a set of instructions, not a finished program, so it needs the interpreter beside it.
- **The same file runs anywhere.** Copy it to Windows, macOS or Linux and it behaves the same way, as long as Python is installed there.

There is one more piece of behaviour that surprises everybody at least once. Before Python runs a single line, it reads the whole file and checks that every line is written correctly. So if you forget one closing bracket on the last line, nothing runs at all — not even the lines above it that were perfectly fine.

This leads to a rule worth keeping. **If a program prints nothing at all, something is written wrongly. If it prints part of its output and then stops, something is wrong in the logic.** Knowing which of the two you are looking at saves a great deal of searching.

## The Reach of the Language

All of that describes how Python runs. It does not explain why this particular language is worth the months you are about to spend on it, and that answer lies outside the language itself.

Python is not a teaching language that you outgrow. The lines you write this week are the same lines written by people doing very different jobs.

| Field of work | What Python is doing there |
|---|---|
| Data analysis | Cleaning large files of records and summarising them |
| Machine learning | Building and training models that make predictions |
| Web development | Running the part of a website that the browser never sees |
| Automation | Replacing work somebody currently does by hand, such as renaming files or producing a monthly report |
| Testing | Checking other software automatically, without a person clicking through it |

One consequence matters for you now. When something goes wrong, somebody has almost certainly hit the same problem and written down the answer, because the number of people using Python is very large. That is not true of every language, and while you are learning it is worth a great deal.

## Installing Python

The interpreter has to be on your machine before any of this can happen, so that is the first practical step.

**On Windows**, open python.org, go to Downloads and run the installer it offers you. On the very first screen, tick **Add python.exe to PATH**, and then click **Install Now**. That one tick is what lets you call Python by name later.

**On macOS**, download the macOS installer from the same page and run it. **On Linux**, Python is usually installed already.

To confirm that it worked, open Command Prompt on Windows or Terminal on macOS and Linux, and type:

```
python --version
```

```
Python 3.14.7
```

On macOS and Linux the command is `python3 --version`. Your version number will probably differ from the one above, and that is fine — anything beginning with 3 will run everything in this course.

If the reply is `'python' is not recognized`, the PATH option was missed during installation. Run the installer once more, choose **Modify**, tick **Add python.exe to PATH**, and finish.

## The First Program

Python is now ready, so it needs something to run. It takes less than you would expect: no special software, no setup, just Notepad.

1. Open Notepad.
2. Type this line exactly as it appears:

```python
print("Welcome to Python")
```

3. Click **File → Save As** and open your **Documents** folder. Create a new folder there named `python-basics` and go inside it.
4. Change **Save as type** from *Text Documents* to **All Files**.
5. Type the file name `hello.py` and save.

Step four is the one that catches people. If **Save as type** is left as *Text Documents*, Notepad quietly saves your file as `hello.py.txt`, and Python will never find it.

Now open Command Prompt. It starts in your own user folder, so two commands are enough to reach the file and run it:

```
cd Documents\python-basics
python hello.py
```

```
Welcome to Python
```

On macOS and Linux the same two commands use a forward slash and `python3`, like `cd Documents/python-basics` followed by `python3 hello.py`. If you kept the folder on another drive on Windows, put the drive in front, as in `cd /d D:\python-basics`.

Look at what the second command is made of. The word `python` starts the interpreter, and `hello.py` tells it which file to work on. The file name itself means nothing to Python — call it `first.py` or `abcd.py` and it behaves exactly the same, because only the code inside it matters.

## Visual Studio Code Setup

Notepad was enough to show that Python works, but it will slow you down from here. It cannot colour your code, it cannot warn you about a missing bracket, and it cannot run a file for you. A code editor does all three, and Visual Studio Code is the usual free choice.

1. Download it from code.visualstudio.com and install it.
2. Press `Ctrl+Shift+X`, search for **Python**, and install the extension published by Microsoft.
3. Click **File → Open Folder** and open your `python-basics` folder.
4. Open `hello.py` and click the run button at the top right.

Make a habit of opening the folder rather than a lone file. Everything you write later assumes that the folder is the place you are working in.

## Google Colab as an Alternative

Not every machine allows new software to be installed, and college and office systems often do not. If that is your situation, open colab.research.google.com and sign in with a Google account. Python runs on Google's servers, so nothing at all is installed on your device.

You type code into a box called a cell and press `Shift+Enter` to run it, and your work is saved to Google Drive on its own.

Two things are worth knowing before you rely on it. Colab does not give you `.py` files, so the command line cannot be practised there, and the session shuts down after a period of inactivity. Use it to keep learning when installation is impossible, and set Python up properly when you can.

## Command Line Basics

Both routes above needed a few typed commands, so the window you typed them into deserves a minute of attention. The command line is simply a place where you tell the computer what to do by typing instead of clicking, and it is where Python programs are normally started. Five commands will carry you for now.

| Purpose | Windows | macOS and Linux |
|---|---|---|
| See which folder you are in | `cd` | `pwd` |
| See what is inside the folder | `dir` | `ls` |
| Go into a folder | `cd python-basics` | `cd python-basics` |
| Come back one folder | `cd ..` | `cd ..` |
| Clear the window | `cls` | `clear` |

Two small habits will save you a lot of typing. Type the first few letters of a folder name and press `Tab`, and the rest of the name appears. Press the up arrow, and the command you last typed comes back.

One message will greet you more often than any other:

```
can't open file 'hello.py': [Errno 2] No such file or directory
```

Nine times out of ten the file is exactly where you left it, and the command line is standing in some other folder. Type `dir` or `ls`, make sure `hello.py` appears in the list, and then run it again.

## Comments in Code

From here the subject shifts from running programs to writing them, and the easiest thing to write is a line that Python pays no attention to. Anything after a `#` on a line is ignored completely.

```python
# Marks of three subjects
total = 240
average = total / 3      # divided by three subjects
```

A comment is there to explain **why** a line exists, not to repeat what the line already shows. Nobody gains anything from `total = 240  # set total to 240`, but a note explaining where the number 240 came from is worth having.

Python has no separate symbol for a comment spread over several lines. When you need one, start every line with a `#`.

## Variables and Assignment

Comments are written for the people who read your code. A variable is written for the program itself.

**A variable is a name that holds a value so that the program can use it again later.** You create one by writing the name, an equals sign, and the value:

```python
student_name = "Kavya"
marks = 88
```

From that point on, `student_name` stands for the text `Kavya` and `marks` stands for the number 88, anywhere in the program.

Four points explain almost everything variables do.

**The equals sign does not mean "equal to".** It means "store this". Python works out the value on the right first, and then attaches the name on the left to it. Read `marks = 88` as *marks takes the value 88*.

**A name can be given a new value at any time, of any kind.** Python never asks you to say in advance what a name will hold.

```python
marks = 88
marks = 91
marks = "not submitted"
```

After the third line, `marks` holds the text `not submitted`, and the two numbers before it are gone for good.

**A name that was never given a value cannot be used.** Python does not treat it as empty or as zero; it stops and tells you the name is unknown.

```python
print(score)
```

```
NameError: name 'score' is not defined
```

**Copying one variable into another copies only the value it holds at that moment.** The two names do not stay linked afterwards.

```python
a = 10
b = a
a = 25
print(b)
```

```
10
```

`b` was given the value 10 while the second line was running. Changing `a` after that has no way of reaching back into `b`.

## Naming Rules for Variables

Since every value you store needs a name, it helps to know which names Python will accept. These four rules are enforced by Python itself, and breaking any of them stops the program.

- **Use only letters, digits and the underscore.** Spaces and symbols such as `-` or `@` are not allowed.
- **A name cannot begin with a digit.** `marks2` is accepted, `2marks` is not.
- **Capital and small letters are treated as different.** `Total`, `total` and `TOTAL` are three separate names.
- **Words that belong to Python cannot be borrowed.** `class = "X"` fails, because `class` is one of Python's own words.

You can see the full list of Python's own words whenever you want:

```python
import keyword
print(keyword.kwlist)
```

The next three points are not rules, and Python will not object if you ignore them. Every Python programmer follows them anyway, and code that ignores them is treated as careless work.

- **Write names in small letters, joining words with an underscore**: `student_name`, `total_marks`.
- **Let the name say what it holds.** `average` tells the reader something; `a` tells them nothing.
- **Never reuse a name that Python already uses.** Write `print = 5` and you lose the ability to print for the rest of that program.

## Data Types

A name is one half of a variable. The value is the other half, and every value in Python belongs to a category.

**That category is called the data type of the value.** It is not a label kept for the reader's benefit — Python looks at the type to decide what is allowed to happen to the value, and the same symbol can mean two different things depending on it. Adding is the clearest case:

```python
print(5 + 3)
print("5" + "3")
```

```
8
53
```

The first `+` added two numbers. The second joined two pieces of text, because text is not something you can add. Python was not confused and did not guess; it read the types and acted accordingly.

Four types cover everything you need at this stage.

**Integer.** A whole number, written without a decimal point.

```python
roll_number = 24
temperature = -5
```

Python places no limit on how large a whole number can be. Even `2 ** 200` is worked out fully and exactly, which is unusual among programming languages.

**Float.** A number with a decimal point.

```python
percentage = 88.5
```

These numbers are stored in a fixed binary form, and a few decimal values simply cannot be held exactly in it:

```python
print(0.1 + 0.2)
```

```
0.30000000000000004
```

This is not a fault in Python. Every mainstream language behaves the same way, for the same reason. What it means in practice is that you round such values before showing them to anyone.

**String.** Text, written inside quotes. Single and double quotes work equally well, as long as you close what you opened.

```python
name = "Anitha"
city = 'Chennai'
```

Digits inside quotes are text and not numbers, which is why `"25" + "30"` gives `"2530"` instead of 55.

**Boolean.** A value that is either `True` or `False`, with nothing in between. Both are written with a capital first letter, and every comparison you make produces one of them.

```python
print(45 > 50)
```

```
False
```

The comparisons available are `>`, `<`, `>=`, `<=`, `==` for *is equal to*, and `!=` for *is not equal to*. Note the two equals signs in `==`. One equals sign stores a value, two compare values, and mixing them up is among the most common mistakes a beginner makes.

## Type Checking and Conversion

Since the type decides what is allowed, there are times when you need to know what you are holding, and times when you need to change it. Python has one instruction for each.

To ask what type a value has:

```python
print(type(24))
print(type(88.5))
print(type("Chennai"))
print(type(True))
```

```
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

To change a value into another type, write the name of the type you want and put the value inside brackets.

| What you want | How to write it | Result |
|---|---|---|
| Text into a whole number | `int("45")` | `45` |
| Text into a decimal number | `float("45.5")` | `45.5` |
| A decimal into a whole number | `int(45.9)` | `45` |
| A number into text | `str(45)` | `"45"` |

Two of these behave in ways that are worth seeing before they cost you marks. `int(45.9)` gives `45`, because the part after the decimal point is thrown away rather than rounded up. And `int("45.5")` does not give `45` at all — it fails:

```
ValueError: invalid literal for int() with base 10: '45.5'
```

`int` accepts text only when the text spells a whole number. Text carrying a decimal point has to go to `float` first. This will matter constantly the moment you start reading values that somebody types in, because everything typed in arrives as text.

## Reading Values from the Keyboard

Every program so far has known its own values in advance, which makes it useless to anybody but you. A program becomes worth running when it asks.

**The `input` instruction stops the program, waits for the person to type something and press Enter, and hands back what they typed.**

```python
name = input("Enter your name: ")
print("Welcome,", name)
```

```
Enter your name: Kavya
Welcome, Kavya
```

The text inside the brackets is the prompt. Always write one. Without it the program simply stops with a blank screen, and the person in front of it has no idea that anything is expected.

Now to the sentence that ended the last section, because it is the single most expensive thing a beginner does not know. **Whatever the person types, `input` hands back text. Even when they type digits.** Adding two of those does not add anything:

```python
first = input("First number: ")
second = input("Second number: ")
print(first + second)
```

```
First number: 10
Second number: 20
1020
```

No error appeared. Python was asked to join two pieces of text and did precisely that. A mistake that produces a wrong answer in silence is far harder to find than one that stops the program, which is why this deserves the attention.

The cure is to convert the moment the value arrives, by wrapping the whole `input` inside `int` or `float`:

```python
first = int(input("First number: "))
second = int(input("Second number: "))
print(first + second)
```

```
First number: 10
Second number: 20
30
```

Use `int` for things that are counted, such as an age or a number of people. Use `float` for things that are measured, such as marks, prices or temperature. When you are unsure, `float` is the safer choice, because it accepts both `85` and `85.5` while `int` refuses the second.

One more thing will happen the first time somebody else uses your program. If they type a word where a number was expected, the program stops:

```
Enter your age: twenty
ValueError: invalid literal for int() with base 10: 'twenty'
```

Guarding against that needs a tool that comes later in the course. For now, assume the person types what you asked for.

## Presenting the Result

A value read in and worked out still has to be shown, and `print` has been doing that since the first program. It has a little more to it than has been used so far.

Given several values at once, it prints them in order and puts a space between them:

```python
print("Total:", 240, "Average:", 80)
```

```
Total: 240 Average: 80
```

That space, and the move to a new line at the end, are both settings you can change. `sep` decides what goes between the values, and `end` decides what goes at the finish:

```python
print("2026", "08", "25", sep="-")
print("Working", end="")
print("... done")
```

```
2026-08-25
Working... done
```

An empty `print()` prints nothing but a blank line, which is the usual way to leave a gap in your output.

Joining text and numbers with `+` is possible, but it forces you to convert every number by hand and the line becomes hard to read:

```python
name = "Kavya"
average = 86.333333
print("Student " + name + " scored " + str(average))
```

```
Student Kavya scored 86.333333
```

There is a far better way. **Put the letter `f` immediately before the opening quote, and anything you write inside curly brackets is worked out and dropped into the text.**

```python
print(f"Student {name} scored {average}")
```

```
Student Kavya scored 86.333333
```

Same result, and the sentence is still readable as a sentence. This is called an f-string, and from here on it is the way to build any line of output. Whole calculations can sit inside the brackets:

```python
print(f"Doubled: {88 * 2}")
```

```
Doubled: 176
```

That leaves one problem: `86.333333` is not a number anybody wants to read. Adding `:.2f` after the value fixes the number of decimal places, and this is also the answer to the rounding oddity seen earlier.

```python
print(f"Average: {average:.2f}")
```

```
Average: 86.33
```

Read `:.2f` as *show this as a decimal number with two places*. You will use it in almost every program that reports a calculated figure.

## Arithmetic Operators

Reading values in and printing them out only becomes useful when something happens in between, and for most beginner programs that something is arithmetic. Python has seven operators.

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `+` | Addition | `7 + 2` | `9` |
| `-` | Subtraction | `7 - 2` | `5` |
| `*` | Multiplication | `7 * 2` | `14` |
| `/` | Division | `7 / 2` | `3.5` |
| `//` | Division, keeping only the whole part | `7 // 2` | `3` |
| `%` | The remainder after division | `7 % 2` | `1` |
| `**` | Raised to the power of | `7 ** 2` | `49` |

The first three hold no surprises. The last four are worth a closer look, because each of them has a habit that catches people out.

**`/` always produces a decimal number, even when the division comes out even.** `10 / 5` gives `2.0`, not `2`, and every calculation that value takes part in afterwards is a decimal one too.

**`//` throws away the fractional part rather than rounding.** `7 // 2` is `3`, and `9 // 2` is `4`. Where a negative number is involved it moves downwards, away from zero, so `-7 // 2` is `-4`.

**`%` is more useful than it first looks.** It answers two everyday questions. `number % 2 == 0` tells you whether a number is even, and dividing a quantity into units relies on it — 7385 seconds is `7385 // 60` whole minutes with `7385 % 60` seconds left over.

**Dividing by zero stops the program.** There is no result Python can give, so it says so:

```python
print(10 / 0)
```

```
ZeroDivisionError: division by zero
```

## The Order of Operations

When several operators appear in one line, the order they are applied in decides the answer, and Python's order is fixed rather than left to right.

Brackets are worked out first. Then powers. Then multiplication and division, including `//` and `%`, taken from left to right. Addition and subtraction come last, also from left to right.

```python
print(2 + 3 * 4)
print((2 + 3) * 4)
print(2 ** 3 ** 2)
```

```
14
20
512
```

The first two are the school rule you already know. The third one is worth pausing on: powers group from the right, so Python worked out `3 ** 2` first and then `2 ** 9`.

The practical advice is short. Wherever a reader might have to stop and think about the order, put brackets in. They cost nothing, they change nothing, and they remove all doubt about what you meant.

## Choosing Between Alternatives

Everything written so far runs from the first line to the last, doing the same thing every time. Real programs behave differently depending on what they find, and that needs a decision.

**A decision is written with `if`, followed by a condition, followed by the lines to run when that condition is true.** `elif` offers another condition to try, and `else` catches everything left over.

```python
marks = 72

if marks >= 90:
    grade = "A"
elif marks >= 75:
    grade = "B"
elif marks >= 60:
    grade = "C"
else:
    grade = "F"

print(grade)
```

```
C
```

Three details in that block are not decoration, and each of them produces an error message when it is missed.

**The colon at the end of every condition line is required.** Leave it out and Python reports `SyntaxError: expected ':'`.

**The four spaces at the start of the next line are required.** That indent is the only thing telling Python which lines belong to the decision. Other languages use brackets for this; Python uses the shape of the text, so the shape is part of the grammar. Pressing `Tab` in Visual Studio Code inserts the four spaces for you.

**Only one branch ever runs.** Python tries the conditions in order and stops at the first one that is true. This is why the example works with such simple conditions: marks of 95 matched the first line and never reached the second, so `elif marks >= 75` did not need to say *and below 90*. It also explains why the conditions must be written from the highest boundary downwards. Turn that block upside down and every student passes with a C.

## Building the Grade Calculator

Everything needed for a real program is now in place: reading values in, converting them, calculating, deciding and printing. Putting them together is the work of this week, and the sensible way to do it is in stages, running the program after each one.

The program has to ask for a student's name and three subject marks, work out the total and the average, and report a grade on this scheme.

| Average | Grade |
|---|---|
| 90 and above | A |
| 75 to 89 | B |
| 60 to 74 | C |
| 40 to 59 | D |
| Below 40 | F |

Start with nothing but the questions, and print the answers straight back:

```python
name = input("Student name: ")
mark1 = float(input("Mark in subject 1: "))
mark2 = float(input("Mark in subject 2: "))
mark3 = float(input("Mark in subject 3: "))

print(name, mark1, mark2, mark3)
```

That last line is temporary. It exists only to prove that four values arrived correctly before anything is built on top of them, and it comes out once the next stage works. `float` is used rather than `int` because a mark of 87.5 is perfectly ordinary.

Now add the arithmetic and check it before going further:

```python
total = mark1 + mark2 + mark3
average = total / 3

print(total, average)
```

```
259.0 86.33333333333333
```

Both numbers are right, and both need tidying before anyone sees them. The grade comes next, written from the highest boundary down:

```python
if average >= 90:
    grade = "A"
elif average >= 75:
    grade = "B"
elif average >= 60:
    grade = "C"
elif average >= 40:
    grade = "D"
else:
    grade = "F"
```

With the temporary prints removed and the output formatted properly, the finished program reads like this:

```python
# grade_calculator.py
# Asks for a student's name and three subject marks, then reports
# the total, the average and the grade.
# The college scheme awards A from 90, B from 75, C from 60 and D from 40.

name = input("Student name: ")
mark1 = float(input("Mark in subject 1: "))
mark2 = float(input("Mark in subject 2: "))
mark3 = float(input("Mark in subject 3: "))

total = mark1 + mark2 + mark3
average = total / 3

if average >= 90:
    grade = "A"
elif average >= 75:
    grade = "B"
elif average >= 60:
    grade = "C"
elif average >= 40:
    grade = "D"
else:
    grade = "F"

print()
print("--- Result ---")
print(f"Name    : {name}")
print(f"Total   : {total:.0f}")
print(f"Average : {average:.2f}")
print(f"Grade   : {grade}")
```

Running it gives:

```
Student name: Kavya Menon
Mark in subject 1: 88
Mark in subject 2: 92
Mark in subject 3: 79

--- Result ---
Name    : Kavya Menon
Total   : 259
Average : 86.33
Grade   : B
```

Note the `:.0f` on the total. Without it the line would read `259.0`, because the marks were read as decimals and so their sum is a decimal too. The value is not wrong; only its appearance is, and that is exactly the kind of thing a format instruction is for.

Once it works, change it. Report the percentage out of 300. Print `Passed` or `Failed` at an average of 40. Extend it to four subjects. Each of those is one or two lines, and making a working program do something new is a different skill from writing one from nothing.

## Keeping a Record of Your Work

A program that works today will be changed tomorrow, and sooner or later a change will break something that used to be fine. Git exists for that moment. It records the state of your project each time you ask it to, so that you can always see what changed and go back if you need to.

Download Git from git-scm.com and install it with the default options. Check it, and tell it who you are, because it writes your name into every record it keeps:

```
git --version
git config --global user.name "Kavya Menon"
git config --global user.email "kavya@example.com"
git config --global init.defaultBranch main
```

The three `config` commands print nothing, which is normal. They are needed only once on a machine, not once per project.

Now go into your project folder and start keeping history:

```
cd Documents\python-basics
git init
git status
```

```
On branch main

No commits yet

Untracked files:
        grade_calculator.py
```

`git status` is the command to run whenever you are unsure of anything. It says what has changed and usually names the command you need next.

Before recording anything, tell Git what to leave alone. Python creates a folder called `__pycache__` for its own use, and that is generated material rather than your work. Create a file named `.gitignore` — the dot is part of the name — containing these three lines:

```
__pycache__/
*.pyc
.venv/
```

Recording your work takes two commands rather than one. `git add` chooses what to record, and `git commit` writes it down:

```
git add grade_calculator.py .gitignore
git commit -m "Add student grade calculator"
```

```
[main (root-commit) 4f2a1c9] Add student grade calculator
 2 files changed, 30 insertions(+)
```

Always include the `-m` and the message. Leave it out and Git opens a text editor inside the command window, which is a confusing place to be stranded on your first day. `4f2a1c9` is the name Git gave this record; yours will be different, because it is worked out from the contents.

From now on the routine is three commands, run every time something works rather than once at the end of the week:

```
git status
git add .
git commit -m "Report percentage out of 300"
```

Write messages that say what changed. `Report percentage out of 300` will still make sense to you in a month. `update` will not.

## Problems to Solve

Reading this file teaches you very little. Writing these five programs teaches you most of it. Keep each one in its own file inside `python-basics`, and commit each one when it works.

**Area and perimeter of a rectangle.** Ask for the length and the breadth, and report both figures to two decimal places.

```
Enter length: 12.5
Enter breadth: 8
Area      : 100.00
Perimeter : 41.00
```

**Seconds into hours and minutes.** Ask for a whole number of seconds and break it into hours, minutes and seconds. This is what `//` and `%` are for.

```
Enter total seconds: 7385
2 hours, 3 minutes, 5 seconds
```

**Simple interest.** Ask for the principal, the yearly rate and the number of years. Interest is the principal multiplied by the rate and the time, divided by 100.

```
Principal amount: 25000
Annual rate (%): 7.5
Time (years): 3
Interest        : 5625.00
Total repayable : 30625.00
```

**Celsius into Fahrenheit.** Convert using nine fifths of the temperature plus thirty-two, and show one decimal place.

```
Temperature in Celsius: 36.6
36.6 C = 97.9 F
```

**Splitting a bill.** Ask for a bill in whole rupees and the number of people sharing it. Report what each person pays and how many rupees cannot be divided evenly.

```
Total bill: 2470
Number of people: 4
Each person pays: 617
Remaining: 2
```

## Errors You Will Meet and Their Causes

Errors are the ordinary condition of writing programs, not a sign that something has gone badly wrong. Python's messages are more helpful than they first appear, and reading the last line first is the habit to build: it names what went wrong, and the line above it names the file and line number where Python stopped.

| Message | What caused it | What to do |
|---|---|---|
| `'python' is not recognized` | Python is missing, or Windows cannot find it | Run the installer again, choose Modify, tick Add python.exe to PATH |
| `can't open file` | The command line is in a different folder from the file | Run `dir` or `ls`, then `cd` into the right folder |
| `SyntaxError: expected ':'` | A colon is missing after an `if`, `elif` or `else` | Add the colon at the end of that line |
| `SyntaxError: invalid syntax` | A bracket or quote was never closed, often on the line above the one reported | Check the previous line |
| `IndentationError` | The line after a colon is not indented | Indent it by four spaces |
| `NameError: name 'x' is not defined` | A misspelling, a wrong capital letter, or a name used before it was given a value | Compare the spelling with where the name was created |
| `TypeError: can only concatenate str` | Text and a number joined with `+` | Use an f-string instead |
| `ValueError: invalid literal for int()` | `int` was given text with a decimal point or with letters in it | Use `float`, or `int(float(value))` |
| `ZeroDivisionError` | Something divided by zero | Check the divisor before dividing |
| Two typed numbers joined instead of adding | `input` was never converted | Wrap it in `int` or `float` |

## Checking Your Own Progress

Nobody is standing over you in self-paced study, so the checking has to be your own. Go through this list on your own machine before starting the next week, because everything that follows assumes all of it is done.

- `python --version` answers with a version beginning with 3.
- A `.py` file runs from the command line with `python filename.py`.
- You can move between folders with `cd`, and list their contents with `dir` or `ls`.
- Visual Studio Code has the Python extension installed and your project folder open.
- `grade_calculator.py` gives the right grade for at least three different sets of marks.
- All five problems above run and produce correct answers.
- `git log --oneline` lists more than one record of your work.

Any line you cannot tick is this week's remaining work, and it is worth going back for. A gap here does not stay where it is — it makes every week after it harder than it needed to be.
