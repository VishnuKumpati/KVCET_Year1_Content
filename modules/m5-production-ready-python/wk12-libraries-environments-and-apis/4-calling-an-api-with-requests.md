# Calling an API with requests

Python's standard library can make HTTP requests through `urllib.request`. Almost nobody uses it directly, because **requests** does the same job far more readably and is the effective standard.

It is not built in, so it needs installing into the project's virtual environment:

```
pip install requests
```

The examples below use https://jsonplaceholder.typicode.com, a free API that serves fake data for practice. It needs no key and can be called freely.

## A First Request

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1", timeout=10)

print(response.status_code)
print(response.text)
```

**Output:**

```
200
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

`requests.get()` sent the request and returned a `Response` object. `status_code` is the number the server replied with, and `text` is the body as a string.

`timeout=10` says to give up after ten seconds. **Always pass it.** Without a timeout, a server that accepts the connection and then never answers leaves the program waiting indefinitely, with no error and no way out. It is the single most commonly omitted argument in `requests` code.

## Parsing JSON

`text` is a string. `json()` parses it into Python objects:

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1", timeout=10)
data = response.json()

print(type(data))
print(data["title"])
print(data["completed"])
```

**Output:**

```
<class 'dict'>
delectus aut autem
False
```

An ordinary dictionary, indexed normally. `false` came back as Python's `False`.

`response.json()` does what `json.loads(response.text)` does. Use the method — it is shorter and handles the encoding for you.

It raises if the body is not JSON, which is covered in the next chapter.

## Checking Before Parsing

A failed request still returns a response:

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/99999999", timeout=10)

print(response.status_code)
print(response.ok)
```

**Output:**

```
404
False
```

No exception was raised. Nothing went wrong from `requests`' point of view — the server was asked a question and gave an answer, and the answer was "no such thing".

`response.ok` is `True` for any status below 400, which makes it a quick guard:

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1", timeout=10)

if response.ok:
    print(response.json()["title"])
else:
    print("Request failed with status", response.status_code)
```

**Output:**

```
delectus aut autem
```

Never call `.json()` without checking first. An error response usually has a body too, and it is not the data you expected.

## Query Parameters

Options go in the query string. Building that string by hand means worrying about `?`, `&` and escaping; `params` takes a dictionary instead:

```python
import requests

response = requests.get(
    "https://jsonplaceholder.typicode.com/todos",
    params={"userId": 1, "completed": "true"},
    timeout=10,
)

print(response.url)
todos = response.json()
print(len(todos))
print(todos[0])
```

**Output:**

```
https://jsonplaceholder.typicode.com/todos?userId=1&completed=true
11
{'userId': 1, 'id': 4, 'title': 'et porro tempora', 'completed': True}
```

`requests` assembled the URL, and `response.url` shows what was actually sent — which is the first thing to print when an API returns something unexpected.

Values containing spaces or symbols are escaped automatically. That alone is reason enough to use `params` rather than an f-string.

This response is a **list** of dictionaries, not a single dictionary. Endpoints that return collections usually do. Check the shape before indexing:

```python
print(type(todos))
print(len(todos))
```

**Output:**

```
<class 'list'>
11
```

## Headers

`headers` takes a dictionary, and is how credentials and content preferences are sent:

```python
import requests

response = requests.get(
    "https://jsonplaceholder.typicode.com/todos/1",
    headers={"Accept": "application/json"},
    timeout=10,
)

print(response.headers["Content-Type"])
```

**Output:**

```
application/json; charset=utf-8
```

`response.headers` is the server's headers; the dictionary you pass is yours. Both behave like dictionaries, though response header names are matched case-insensitively.

An API key goes here, read from the environment rather than written in the file:

```python
import os
import requests

api_key = os.environ.get("EXAMPLE_API_KEY")

response = requests.get(
    "https://api.example.com/v1/data",
    headers={"Authorization": f"Bearer {api_key}"},
    timeout=10,
)
```

## Sending Data

`requests.post()` sends a body. The `json` argument serialises a dictionary and sets `Content-Type` automatically:

```python
import requests

response = requests.post(
    "https://jsonplaceholder.typicode.com/posts",
    json={"title": "Marks report", "body": "All submitted", "userId": 1},
    timeout=10,
)

print(response.status_code)
print(response.json())
```

**Output:**

```
201
{'title': 'Marks report', 'body': 'All submitted', 'userId': 1, 'id': 101}
```

`201 Created` rather than `200`, and the response includes the `id` the server assigned. That is the usual pattern: send the fields, get back the stored record.

Note `json=` rather than `data=`. `data=` sends form-encoded content, which is what an HTML form submits; `json=` sends a JSON body, which is what most APIs expect. Using the wrong one produces a `400`.

The other methods work the same way:

```python
requests.put(url, json=payload, timeout=10)
requests.patch(url, json=payload, timeout=10)
requests.delete(url, timeout=10)
```

## The Response Object

| Attribute | Gives |
| --- | --- |
| `status_code` | the status as an integer |
| `ok` | `True` if the status is below 400 |
| `text` | the body as a string |
| `content` | the body as bytes, for images and files |
| `json()` | the body parsed from JSON |
| `headers` | the response headers |
| `url` | the URL actually requested |
| `elapsed` | how long the request took |
| `request` | the request that was sent |

`response.request.headers` shows what your program sent, which is how to check that a header or key actually went out.

## A Complete Example

Fetching, filtering and summarising, with the pieces from earlier modules doing the work:

```python
import requests

response = requests.get(
    "https://jsonplaceholder.typicode.com/todos",
    params={"userId": 1},
    timeout=10,
)

if not response.ok:
    print("Request failed with status", response.status_code)
else:
    todos = response.json()
    done = [todo for todo in todos if todo["completed"]]

    print(f"{len(todos)} tasks, {len(done)} completed")
    print(f"Completion: {len(done) / len(todos):.0%}")
    print("First completed:", done[0]["title"])
```

**Output:**

```
20 tasks, 11 completed
Completion: 55%
First completed: et porro tempora
```

The response became a list of dictionaries, and from there it is the data wrangling already covered — a comprehension to filter, `len()` to count, an f-string to report.

## Be a Considerate Client

Every request costs someone else's server time and bandwidth.

**Do not call an API inside a tight loop** without pausing. A loop making a thousand requests as fast as possible looks like an attack and will get your address blocked.

**Fetch once and reuse.** If the same data is needed several times, store it in a variable — or a file — rather than requesting it again.

**Set a `User-Agent`** identifying your program, so an administrator seeing unusual traffic can tell what it is.

**Read the terms.** Many APIs specify how often they may be called and what the data may be used for.

The next chapter covers what to do when a request fails, including when a server says you are calling too often.

## Further Reading

- **The requests documentation** — https://requests.readthedocs.io/
- **Python's requests library** — https://realpython.com/python-requests/

`requests.get()` with `params`, `headers` and always a `timeout`, then check `ok` before calling `json()`. Next, everything that can go wrong with that.
