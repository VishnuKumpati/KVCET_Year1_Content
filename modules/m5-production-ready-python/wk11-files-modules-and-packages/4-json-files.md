# JSON Files

CSV holds a flat table of text. **JSON** — JavaScript Object Notation — holds structure and types: nested objects, lists, numbers, booleans and null.

It is the standard format for configuration files and for data sent between programs over the internet, which makes it the format you will meet most often after CSV.

The syntax is close enough to Python's that it barely needs learning:

```json
{
  "name": "Anita",
  "age": 21,
  "marks": [78, 91, 64],
  "active": true,
  "grade": null
}
```

Three differences from Python: `true` and `false` are lowercase, `None` is written `null`, and keys must be double-quoted strings.

## Four Functions

The `json` module has two pairs, differing only in whether they work on a file or a string:

| Function | Direction | Works on |
| --- | --- | --- |
| `json.dump(obj, file)` | Python → JSON | a file |
| `json.load(file)` | JSON → Python | a file |
| `json.dumps(obj)` | Python → JSON | a string |
| `json.loads(text)` | JSON → Python | a string |

The `s` stands for string. Everything else is the same.

## Converting to a String

`dumps()` turns a Python object into JSON text:

```python
import json

record = {"name": "Anita", "age": 21, "marks": [78, 91], "active": True, "grade": None}
print(json.dumps(record))
```

**Output:**

```
{"name": "Anita", "age": 21, "marks": [78, 91], "active": true, "grade": null}
```

`True` became `true` and `None` became `null`. The result is a string — not a dictionary that happens to print that way.

`indent` formats it for reading:

```python
import json

record = {"name": "Anita", "age": 21, "marks": [78, 91]}
print(json.dumps(record, indent=2))
```

**Output:**

```
{
  "name": "Anita",
  "age": 21,
  "marks": [
    78,
    91
  ]
}
```

Use `indent` for anything a person will open — a configuration file, a saved report. Leave it off for data being sent somewhere, where the extra whitespace is wasted.

`sort_keys=True` orders the keys alphabetically, which makes two versions of a file comparable:

```python
import json

record = {"name": "Anita", "age": 21, "active": True}
print(json.dumps(record, indent=2, sort_keys=True))
```

**Output:**

```
{
  "active": true,
  "age": 21,
  "name": "Anita"
}
```

## Converting from a String

`loads()` parses JSON text into Python objects:

```python
import json

text = '{"name": "Anita", "age": 21, "marks": [78, 91], "active": true}'
record = json.loads(text)

print(type(record))
print(record)
print(record["marks"][1])
print(record["active"], type(record["active"]))
```

**Output:**

```
<class 'dict'>
{'name': 'Anita', 'age': 21, 'marks': [78, 91], 'active': True}
91
True <class 'bool'>
```

Note the single quotes in the second line: that is a real dictionary being printed, not JSON. Indexing works normally from there, and `true` came back as Python's `True`.

The top level need not be an object:

```python
import json
print(json.loads("[1, 2, 3]"))
print(json.loads("true"), type(json.loads("true")))
```

**Output:**

```
[1, 2, 3]
True <class 'bool'>
```

## Files

`dump()` and `load()` take an open file instead of returning or accepting a string:

```python
import json

record = {"name": "Anita", "age": 21, "marks": [78, 91]}

with open("record.json", "w", encoding="utf-8") as handle:
    json.dump(record, handle, indent=2)

with open("record.json", encoding="utf-8") as handle:
    loaded = json.load(handle)

print(loaded)
print(loaded == record)
```

**Output:**

```
{'name': 'Anita', 'age': 21, 'marks': [78, 91]}
True
```

The round trip returned an equal object. That is the property that makes JSON useful for saving state: what goes out comes back.

Note the argument order. `json.dump(data, file)` — the data first, the file second. Getting it backwards is a common slip.

## The Type Mapping

| Python | JSON |
| --- | --- |
| `dict` | object |
| `list`, `tuple` | array |
| `str` | string |
| `int`, `float` | number |
| `True` / `False` | `true` / `false` |
| `None` | `null` |

The mapping is not symmetrical, and two rows explain most surprises.

**A tuple becomes an array, and comes back a list.** JSON has one sequence type:

```python
import json
print(json.loads(json.dumps((1, 2, 3))))
```

**Output:**

```
[1, 2, 3]
```

**Keys become strings.** JSON object keys are always strings, so a dictionary keyed by numbers does not survive:

```python
import json

original = {1: "a", 2: "b"}
text = json.dumps(original)
print(text)
print(json.loads(text))
```

**Output:**

```
{"1": "a", "2": "b"}
{'1': 'a', '2': 'b'}
```

The keys went in as integers and came back as strings. Use string keys in anything destined for JSON.

## What JSON Cannot Hold

Types outside the table raise an error rather than being guessed at:

```python
import json
print(json.dumps({"tags": {"python", "beginner"}}))
```

**Output:**

```
TypeError: Object of type set is not JSON serializable
```

Sets, dates, and your own class instances are all unserializable. Convert them first — a set to a list, a date to a string — and convert back after loading.

## Invalid JSON

Parsing malformed text raises `JSONDecodeError`:

```python
import json
json.loads("{bad}")
```

**Output:**

```
JSONDecodeError: Expecting property name enclosed in double quotes: line 1 column 2 (char 1)
```

The message gives the line, column and character position, which is usually enough to find the problem in a file.

`JSONDecodeError` is a kind of `ValueError`, so either catches it:

```python
import json

def load_settings(path):
    try:
        with open(path, encoding="utf-8") as handle:
            return json.load(handle)
    except FileNotFoundError:
        print("No settings file; using defaults")
        return {}
    except json.JSONDecodeError as error:
        print(f"Settings file is not valid JSON: {error}")
        return {}

print(load_settings("missing.json"))
```

**Output:**

```
No settings file; using defaults
{}
```

Two failures, two distinct messages. A missing configuration file is normal; a corrupt one is a different problem, and the person running the program should be told which.

## A Configuration File

The pattern JSON is used for most often:

```python
import json
from pathlib import Path

DEFAULTS = {"theme": "light", "font_size": 12, "recent_files": []}

def load_config(path):
    if not Path(path).exists():
        return DEFAULTS.copy()
    with open(path, encoding="utf-8") as handle:
        stored = json.load(handle)
    settings = DEFAULTS.copy()
    settings.update(stored)
    return settings

def save_config(path, settings):
    with open(path, "w", encoding="utf-8") as handle:
        json.dump(settings, handle, indent=2)

config = load_config("config.json")
config["theme"] = "dark"
config["recent_files"].append("notes.txt")
save_config("config.json", config)

print(json.dumps(load_config("config.json"), indent=2))
```

**Output:**

```
{
  "theme": "dark",
  "font_size": 12,
  "recent_files": [
    "notes.txt"
  ]
}
```

`DEFAULTS.copy()` then `update()` is what makes this robust: a settings file missing a key still produces a complete configuration, so adding a new setting in a later version does not break existing files.

## CSV or JSON

| | CSV | JSON |
| --- | --- | --- |
| Shape | flat table | nested structure |
| Types | everything is text | numbers, booleans, null preserved |
| Readable in a spreadsheet | yes | no |
| Good for | rows of records | configuration, API data, nested records |

Use CSV for a table someone may open in a spreadsheet. Use JSON for structure, for types that must survive, and for anything exchanged with a web service.

## Further Reading

- **Official reference for the `json` module** — https://docs.python.org/3/library/json.html
- **Working with JSON data in Python** — https://realpython.com/python-json/

`dump` and `load` for files, `dumps` and `loads` for strings, with types preserved except tuples and non-string keys. Next, splitting a program across more than one file.
