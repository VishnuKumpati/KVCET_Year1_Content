# Data Wrangling

Nothing new is introduced here. One set of records is carried through a complete piece of work, and at each step the tool that fits best is the one used.

**Data wrangling** is the work between receiving data and being able to answer a question with it: finding out what shape it is in, correcting what is wrong with it, selecting what matters, reshaping it, and summarising it. In practice it takes far more of a programmer's time than the calculation at the end.

## The Data

A list of dictionaries, as records arrive from a file or an API — and as messy as they usually are:

```python
orders = [
    {"id": 1, "customer": "Anita ", "region": "North", "amount": 1200, "status": "shipped"},
    {"id": 2, "customer": "ravi", "region": "south", "amount": 450, "status": "pending"},
    {"id": 3, "customer": "Meera", "region": "North", "amount": None, "status": "shipped"},
    {"id": 4, "customer": "  Sunil", "region": "East", "amount": 890, "status": "cancelled"},
    {"id": 5, "customer": "anita", "region": "NORTH", "amount": 300, "status": "shipped"},
    {"id": 6, "customer": "Ravi", "region": "South", "amount": 2100, "status": "shipped"},
    {"id": 7, "customer": "Meera", "region": "east", "status": "shipped"},
]
```

Four problems are already visible, and finding them is the first job rather than something to assume.

## Step 1: Survey

Before anything else, find out what is there. How many records, and what fields does one have:

```python
print(len(orders))
print(sorted(orders[0].keys()))
```

**Output:**

```
7
['amount', 'customer', 'id', 'region', 'status']
```

Never trust one record to describe them all:

```python
print(sorted(orders[6].keys()))
```

**Output:**

```
['customer', 'id', 'region', 'status']
```

The last record has no `amount` at all. Fields that are optional in the source are the commonest cause of `KeyError` later, and the survey is where they should be discovered.

A set comprehension over a field shows its distinct values, which is the quickest way to spot inconsistency:

```python
print(sorted({order["status"] for order in orders}))
print(sorted({order["region"] for order in orders}))
```

**Output:**

```
['cancelled', 'pending', 'shipped']
['East', 'NORTH', 'North', 'South', 'east', 'south']
```

`status` is clean: three values, consistently spelled. `region` is not. Six distinct values for what are obviously three regions, differing only in case. Grouping by region as it stands would produce six groups and three wrong answers.

Finally, locate the missing values rather than just counting them:

```python
print([order["id"] for order in orders if order.get("amount") is None])
```

**Output:**

```
[3, 7]
```

`get()` is what makes this work. `order["amount"]` would raise `KeyError` on record 7 — the very record being looked for. `get()` returns `None` both for a field that is absent and for one explicitly set to `None`, which is what the question asks about.

## Step 2: Clean

Fix everything found in the survey, in one place. A named function per record keeps the rules visible and testable:

```python
def clean(order):
    return {
        "id": order["id"],
        "customer": order["customer"].strip().title(),
        "region": order["region"].strip().title(),
        "amount": order.get("amount") or 0,
        "status": order["status"].strip().lower(),
    }

tidy = [clean(order) for order in orders]
print(tidy[0])
```

**Output:**

```
{'id': 1, 'customer': 'Anita', 'region': 'North', 'amount': 1200, 'status': 'shipped'}
```

Three decisions are worth naming.

`strip().title()` chains two string methods, each returning a new string for the next to work on. `"  Sunil"` and `"anita"` both become `"Anita"`-style text, so the same person is no longer two people.

`order.get("amount") or 0` handles the missing field and the explicit `None` with one expression. `get()` returns `None` in both cases, and `or` yields the right-hand side when the left is falsy. Note the side effect: an amount of `0` is also falsy, so it too becomes `0`. That is harmless here and would not be if the default were something other than zero — in which case `order.get("amount")` compared against `None` explicitly is the safer form.

The original list is untouched. `clean()` builds a new dictionary rather than modifying the one passed in, so `orders` still holds the raw data if the cleaning rules turn out to be wrong.

The survey, run again, confirms the fix:

```python
print(sorted({order["region"] for order in tidy}))
print(sorted({order["customer"] for order in tidy}))
```

**Output:**

```
['East', 'North', 'South']
['Anita', 'Meera', 'Ravi', 'Sunil']
```

Six regions became three and the duplicate customers merged.

## Step 3: Filter

Keep the records the question is about. A list comprehension with a condition:

```python
shipped = [order for order in tidy if order["status"] == "shipped"]
print(len(shipped))
print([order["id"] for order in shipped])
```

**Output:**

```
5
[1, 3, 5, 6, 7]
```

Five of seven. The pending and cancelled orders are gone.

Filtering after cleaning is deliberate. `order["status"] == "shipped"` is a reliable test only because `status` was already lowercased and stripped; against the raw data a stray capital would have silently dropped a record.

## Step 4: Sort

`sorted()` with a key, largest first:

```python
by_amount = sorted(shipped, key=lambda order: order["amount"], reverse=True)
for order in by_amount:
    print(f"{order['id']:>3}  {order['customer']:<10}{order['region']:<8}{order['amount']:>8,}")
```

**Output:**

```
  6  Ravi      South      2,100
  1  Anita     North      1,200
  5  Anita     North        300
  3  Meera     North          0
  7  Meera     East           0
```

The whole records were sorted, not just the amounts, so every field is still available for the report. The `,` in the format specification groups the thousands, and the widths line the columns up.

## Step 5: Group

Splitting records into groups is the one step with no comprehension form, because each group accumulates several records. A loop with `setdefault()` does it:

```python
by_region = {}
for order in shipped:
    by_region.setdefault(order["region"], []).append(order)

print({region: [order["id"] for order in group] for region, group in by_region.items()})
```

**Output:**

```
{'North': [1, 3, 5], 'South': [6], 'East': [7]}
```

A list of records went in and a dictionary of lists came out, keyed by region. `setdefault(region, [])` returned the existing list for a region already seen and created an empty one otherwise, so no check for a first occurrence was needed.

The `print()` uses a dictionary comprehension purely to make the output readable — the groups themselves hold whole records, not ids.

## Step 6: Summarise

With the records grouped, totals and averages are dictionary comprehensions over the groups:

```python
totals = {region: sum(order["amount"] for order in group)
          for region, group in by_region.items()}

averages = {region: sum(order["amount"] for order in group) / len(group)
            for region, group in by_region.items()}

print(totals)
print(averages)
```

**Output:**

```
{'North': 1500, 'South': 2100, 'East': 0}
{'North': 500.0, 'South': 2100.0, 'East': 0.0}
```

Note the round brackets inside `sum()`. That is a generator expression, and it is the right choice here: the amounts are added up once and never looked at again, so there is no reason to build a list of them.

`len(group)` as the divisor is safe because `setdefault()` only creates a key when a record is being appended to it. No group can be empty, so no division by zero is possible.

Summarising by a different field does not need the grouping step at all, when only one number per key is wanted. The accumulator pattern with `get()` builds the totals directly:

```python
customer_totals = {}
for order in shipped:
    customer_totals[order["customer"]] = customer_totals.get(order["customer"], 0) + order["amount"]
print(customer_totals)
```

**Output:**

```
{'Anita': 1500, 'Meera': 0, 'Ravi': 2100}
```

Anita's two orders combined, which they would not have done before the cleaning step.

`max()` with a key finds the largest. Passing `customer_totals.get` as the key looks up each customer's total:

```python
best = max(customer_totals, key=customer_totals.get)
print(best, customer_totals[best])
```

**Output:**

```
Ravi 2100
```

Looping over a dictionary gives keys, so `max()` would compare the customer names — and `key=customer_totals.get` tells it to compare their totals instead while still returning the name.

## Step 7: Report

f-strings with widths and format specifications turn the summaries into something readable:

```python
print(f"{'Region':<10}{'Orders':>8}{'Total':>12}{'Average':>12}")
for region in sorted(totals):
    print(f"{region:<10}{len(by_region[region]):>8}{totals[region]:>12,}{averages[region]:>12,.2f}")
print(f"{'All':<10}{len(shipped):>8}{sum(totals.values()):>12,}{sum(totals.values()) / len(shipped):>12,.2f}")
```

**Output:**

```
Region      Orders       Total     Average
East             1           0        0.00
North            3       1,500      500.00
South            1       2,100    2,100.00
All              5       3,600      720.00
```

`sorted(totals)` orders the report by region name without disturbing the dictionary. Text is left-aligned, numbers right-aligned, and the totals row recomputes from the same data rather than being accumulated separately.

## The Whole Thing as a Pipeline

Each step above used one tool for one job. Written together, with the cleaning already done, a complete query is three lines:

```python
result = sorted(
    ({"customer": order["customer"], "amount": order["amount"]}
     for order in tidy
     if order["status"] == "shipped" and order["amount"] > 0),
    key=lambda order: order["amount"],
    reverse=True,
)
print(result)
```

**Output:**

```
[{'customer': 'Ravi', 'amount': 2100}, {'customer': 'Anita', 'amount': 1200}, {'customer': 'Anita', 'amount': 300}]
```

A generator expression filters and reshapes, `sorted()` with a key orders the result, and `sorted()` consumes the generator once — so nothing but the final list is ever held.

Questions about it are one line each:

```python
print(sum(order["amount"] for order in result))
print(any(order["amount"] > 2000 for order in result))
print(all(order["amount"] > 100 for order in result))
```

**Output:**

```
3600
True
True
```

Write it this way when the pipeline is short and each step is obvious. Write it as separate named steps when it is not — `shipped`, `by_region` and `totals` each name something a reader can check, and a bug in a three-line expression is much harder to find than a bug in one of six lines.

## Choosing the Tool for the Step

| Step | Tool |
| --- | --- |
| Survey distinct values | a set comprehension |
| Find missing fields | `get()` in a comprehension's condition |
| Clean each record | a named function, applied by a list comprehension |
| Filter | a list comprehension with `if` |
| Transform a field | a comprehension's expression |
| Sort | `sorted()` with `key=` |
| Group | a loop with `setdefault()` |
| Total or average | a dictionary comprehension over the groups |
| Aggregate one number | `sum()`, `max()` or `len()` over a generator expression |
| Test a condition across records | `any()` or `all()` over a generator expression |
| Report | f-strings with widths and format specifications |

Two entries are worth remembering because they are the ones people reach for wrongly. Grouping needs a loop — a comprehension produces one value per item and has nowhere to accumulate. Sorting needs `key=` — no comprehension reorders anything.

## Handling What Is Wrong with Real Data

Four faults appeared in seven records, and they are the four that appear in almost any dataset.

**A missing field.** Use `get()` with a default that behaves sensibly in the code that follows — `0` for a number to be summed, `[]` for something to be looped over, `""` for text. A default chosen this way removes every downstream check.

**An explicit null.** `None` is not the same as absent, and `get()` returns it as a real value. `order.get("amount") or 0` covers both; comparing against `None` covers both while leaving a genuine `0` alone.

**Inconsistent formatting.** Case and stray whitespace are the usual offenders, and they matter because every grouping, matching and counting operation compares exact values. Normalise once, at the cleaning step, and everything afterwards can assume it.

**Duplicates that are not identical.** `"Anita "`, `"anita"` and `"Anita"` are three strings and one customer. Normalising is what makes them one, and the set comprehension in the survey is what reveals the problem in the first place.

The order matters. Survey, then clean, then everything else. Filtering or grouping before cleaning gives answers that look reasonable and are wrong, which is worse than an error.

## Further Reading

- **Official Python guide to data structures** — https://docs.python.org/3/tutorial/datastructures.html
- **Official Python sorting guide** — https://docs.python.org/3/howto/sorting.html

Comprehensions filter and reshape, `sorted()` with a key orders, `setdefault()` groups, generator expressions aggregate without building anything, and f-strings present the result. Together they take raw records to a finished answer, and the same sequence applies whatever the records happen to be.
