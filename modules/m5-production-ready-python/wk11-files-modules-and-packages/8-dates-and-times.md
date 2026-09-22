# Dates and Times

Dates look simple and are not. Months have different lengths, years occasionally have an extra day, and the same moment is written five different ways depending on who is reading. A date held as the string `"16-09-2026"` cannot be compared, cannot be added to, and cannot be sorted correctly. The `datetime` module provides real date and time values that can.

```python
from datetime import date

day = date(2026, 9, 16)
print(day)
print(day.year, day.month, day.day)
print(day.weekday())
```

**Output:**

```
2026-09-16
2026 9 16
2
```

Arguments go in the order year, month, day — the same order the default printed form uses. `weekday()` numbers the days from Monday as `0`, so `2` is a Wednesday.

An impossible date is rejected as it is built, not later:

```python
from datetime import date

print(date(2026, 2, 30))
```

**Output:**

```
ValueError: day is out of range for month
```

## The Four Types

| Type | Holds |
| --- | --- |
| `date` | year, month, day |
| `time` | hour, minute, second |
| `datetime` | both together |
| `timedelta` | a length of time, not a point in it |

```python
from datetime import date, time, datetime

print(date(2026, 9, 16))
print(time(14, 30, 5))
print(datetime(2026, 9, 16, 14, 30, 5))
```

**Output:**

```
2026-09-16
14:30:05
2026-09-16 14:30:05
```

The name `datetime` is both the module and one of the types inside it, which is why the import line reads `from datetime import datetime`. It looks like a mistake and is not.

## Now

Two functions give the current moment:

```python
from datetime import date, datetime

today = date.today()
now = datetime.now()

print(type(today))
print(type(now))
print(now.hour)
```

**Output:**

```
<class 'datetime.date'>
<class 'datetime.datetime'>
14
```

Their values differ every run, so the examples below use fixed dates instead. That is worth copying: code that calls `now()` deep inside a function is hard to test, because the answer changes. Taking the current moment once at the top and passing it in leaves the rest of the code predictable.

## Formatting for Display

The default printed form is the ISO form, `2026-09-16`. `strftime()` produces any other, using a code for each part:

```python
from datetime import datetime

moment = datetime(2026, 9, 16, 14, 30, 5)

print(moment.strftime("%d/%m/%Y"))
print(moment.strftime("%A %d %B %Y"))
print(moment.strftime("%d %b %Y, %I:%M %p"))
```

**Output:**

```
16/09/2026
Wednesday 16 September 2026
16 Sep 2026, 02:30 PM
```

| Code | Means | Example |
| --- | --- | --- |
| `%Y` | four-digit year | `2026` |
| `%y` | two-digit year | `26` |
| `%m` | month as a number | `09` |
| `%B` | month name | `September` |
| `%b` | short month name | `Sep` |
| `%d` | day of the month | `16` |
| `%A` | weekday name | `Wednesday` |
| `%H` | hour, 24-hour clock | `14` |
| `%I` | hour, 12-hour clock | `02` |
| `%M` | minute | `30` |
| `%S` | second | `05` |
| `%p` | AM or PM | `PM` |

Anything in the string that is not a code is copied through, which is how the slashes, commas and spaces above get there.

## Reading a Date From Text

`strptime()` goes the other way, turning text into a `datetime` using the same codes to say what the text looks like:

```python
from datetime import datetime

parsed = datetime.strptime("16-09-2026", "%d-%m-%Y")
print(parsed)
print(parsed.month)
```

**Output:**

```
2026-09-16 00:00:00
9
```

The result is a `datetime`, so the missing time is filled in as midnight. Call `.date()` on it when only the date is wanted.

The two names are easy to confuse. **`strftime` formats; `strptime` parses.** The `f` is for format and the `p` is for parse.

The pattern must match the text exactly, separators included:

```python
from datetime import datetime

print(datetime.strptime("16/09/2026", "%d-%m-%Y"))
```

**Output:**

```
ValueError: time data '16/09/2026' does not match format '%d-%m-%Y'
```

That error is common when reading dates out of a file, where one row is written differently from the rest. Wrapping the call in `try` and `except ValueError` lets the bad row be reported rather than stopping the program.

For text already in the standard form, `fromisoformat()` is shorter and needs no pattern:

```python
from datetime import date

print(date.fromisoformat("2026-09-16"))
```

**Output:**

```
2026-09-16
```

## Arithmetic

`timedelta` is a length of time. Adding one to a date gives another date:

```python
from datetime import date, timedelta

day = date(2026, 9, 16)

print(day + timedelta(days=30))
print(day - timedelta(weeks=2))
print(day + timedelta(days=365))
```

**Output:**

```
2026-10-16
2026-09-02
2027-09-16
```

Month lengths and leap years are handled for you. That is the whole reason not to do this arithmetic by hand.

Subtracting two dates gives the `timedelta` between them:

```python
from datetime import date

exam = date(2026, 12, 25)
today = date(2026, 9, 16)

gap = exam - today
print(gap)
print(gap.days)
```

**Output:**

```
100 days, 0:00:00
100
```

`.days` is the whole number of days. A `timedelta` from two `datetime` values also carries `.seconds` for the part-day remainder.

A `timedelta` accepts `days`, `weeks`, `hours`, `minutes` and `seconds` — but deliberately not `months` or `years`, because neither has a fixed length. "One month after 31 January" has no single correct answer, so the module refuses to guess.

## Comparing and Sorting

Date values compare with the ordinary operators:

```python
from datetime import date

due = date(2026, 9, 30)
submitted = date(2026, 10, 2)

print(submitted > due)
print((submitted - due).days, "days late")
```

**Output:**

```
True
2 days late
```

And because they compare, they sort correctly:

```python
from datetime import date

dates = [date(2026, 12, 1), date(2026, 3, 15), date(2026, 9, 16)]
for d in sorted(dates):
    print(d.strftime("%d %b %Y"))
```

**Output:**

```
15 Mar 2026
16 Sep 2026
01 Dec 2026
```

Sorting the strings `"01/12/2026"`, `"15/03/2026"` and `"16/09/2026"` would have put them in the wrong order, because a string sorts left to right and the day comes first. This is the practical argument for parsing dates on the way in: **parse at the edge, work with real date values, format only on the way out.**

## Time Zones

The values built so far are *naive* — they carry no time zone, so they mean whatever the reader assumes. That is fine for a date of birth or a due date, and wrong for anything shared across places.

```python
from datetime import datetime, timezone

aware = datetime.now(timezone.utc)
print(aware.tzinfo)
```

**Output:**

```
UTC
```

The working rule for anything recorded or exchanged: store in UTC, convert to local time only for display. Mixing naive and aware values raises `TypeError` rather than quietly producing a wrong answer, which is the module protecting you.

## Summary

| Need | Use |
| --- | --- |
| Build a date | `date(y, m, d)` |
| Today, now | `date.today()`, `datetime.now()` |
| Date to text | `value.strftime(pattern)` |
| Text to date | `datetime.strptime(text, pattern)` |
| Standard text to date | `date.fromisoformat(text)` |
| Shift a date | `value + timedelta(days=n)` |
| Days between | `(later - earlier).days` |
| Compare, sort | `<`, `>`, `sorted()` |

## Further Reading

- **Official `datetime` reference** — https://docs.python.org/3/library/datetime.html
- **The `strftime` and `strptime` codes** — https://strftime.org/
- **Working with dates in Python** — https://realpython.com/python-datetime/

Dates held as real values compare, sort and add correctly, and text is only ever the form they take on the way in and out. That completes what Python provides for handling data on its own.
