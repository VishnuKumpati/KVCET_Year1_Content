# Calling an API with `requests`

A web browser can send a GET request and display the response. But what if you want to fetch the data automatically, select only the information you need, calculate with it, or save it to a file?

A Python program can do all this by sending requests to an API and processing the responses it receives.

Python can send HTTP requests using tools in its standard library, but for many everyday API tasks, an external library called **`requests`** makes the code simpler.

> **`requests` is a Python library for sending HTTP requests and working with the responses. It lets you call a web API with just a few lines of code.**

This topic uses **JSONPlaceholder**, the same practice API used in **Web APIs and API Keys**, so every response will look familiar.

## Installing `requests`

`requests` is an external library, so it must be installed first. Open a terminal in your project folder, activate the project's virtual environment as explained in **Virtual Environments and pip**, and run:

```
python -m pip install requests
```

Downloading needs an internet connection. Every example in this topic also needs an internet connection, because it sends requests to a real API.

To check the installation, run this in Python:

```python
import requests

print(requests.__version__)
```

**Possible output:**

```
A version number, such as 2.x.x
```

If this prints a version number, `requests` is installed. Your version may be different. If Python shows `ModuleNotFoundError`, check that the virtual environment is active and install `requests` again.

## Sending a GET Request

The `requests.get()` function sends a **GET** request to a URL and returns the server's response.

**Syntax:**

```
response = requests.get(url)
```

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1")

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

- `requests.get()` sends the same GET request your browser sent in **Web APIs and API Keys**.
- The server's reply is stored in `response`. `response` is just a **variable name** chosen by the programmer, like `url` or `todo` later in this topic. Any valid name would work.
- `response.status_code` is `200`, which means the server reported that the request succeeded.
- `response.text` is the response body as one string.

> **Practical note:** These introductory examples leave out a **timeout** to keep the code simple. In real programs, use a timeout such as `requests.get(url, timeout=10)`, so your program does not wait forever if the server does not respond. Timeouts are explained in **Handling API Errors**.

We have received a response, but how do we inspect it and get the information we need? The `Response` object provides several attributes and methods for this.

## The Response Object

`requests.get()` returns a **`Response` object**.

> **A `Response` object stores everything the server sent back: the status code, the headers and the body.**

In Python, an **attribute** gives you information about an object, while a **method** is an operation you can call on it. For example, `response.status_code` reads an attribute, whereas `response.json()` calls a method, which is why it has parentheses.

The most useful attributes and methods of a `Response` object are:

| Attribute or Method | What It Gives | Example Value |
| --- | --- | --- |
| `response.status_code` | The status code, as a number | `200` |
| `response.ok` | `True` if the final status code is below 400; otherwise, `False` | `True` |
| `response.text` | The body, as one string | `'{"userId": 1, ...}'` |
| `response.json()` | The body converted into Python data | `{'userId': 1, ...}` |
| `response.headers` | The response headers, like a dictionary | `{'Content-Type': ...}` |
| `response.url` | The full URL the request was sent to | `'https://...'` |

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1")

print(response.ok)
print(response.headers["Content-Type"])
```

**Output:**

```
True
application/json; charset=utf-8
```

- `response.ok` is `True` when the final status code is below `400`. Check `response.status_code` when you need to know the exact result.
- The `Content-Type` header shows that the body contains JSON data.

## Reading JSON Data

In the first example, `response.text` gave the entire response body as one string. But what if you want only the task's title or its completion status? You can convert the JSON response into Python data using the **`json()`** method, which works like `json.loads()` in **JSON Files**, and then access individual values.

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/1")
todo = response.json()

print(type(todo))
print(todo["title"])
print(todo["completed"])
```

**Output:**

```
<class 'dict'>
delectus aut autem
False
```

- `response.json()` converts a valid JSON response body into the matching Python data type. In this example, the JSON **object** becomes a Python **dictionary**.
- JSON is a **format** for exchanging data between systems, while a dictionary is a **Python data type**. When Python reads JSON, it converts JSON objects into dictionaries and JSON arrays into lists.
- `todo["title"]` reads one value using its key, like any dictionary.
- JSON's `false` becomes Python's `False`.

If the body contains a JSON **array**, `json()` returns a Python **list** instead.

**Successfully converting JSON does not mean the HTTP request succeeded.** Check the status code separately.

`json()` works only when the response body contains valid JSON. If it does not, `json()` raises an error, which is covered in **Handling API Errors**.

Sometimes an API returns a collection of records, but you need only the records that match certain conditions. Instead of getting everything and filtering it yourself, you can send **query parameters** to ask the API for specific data.

## Sending Query Parameters

Query parameters can be written into the URL by hand, such as `?userId=1&completed=true`. It is clearer to pass them as a dictionary using the **`params`** argument. `requests` then builds the query string for you.

```python
import requests

url = "https://jsonplaceholder.typicode.com/todos"
parameters = {"userId": 1, "completed": "true"}

response = requests.get(url, params=parameters)
todos = response.json()

print(response.url)
print(len(todos))

for todo in todos[:3]:
    print(todo["id"], todo["title"])
```

**Output:**

```
https://jsonplaceholder.typicode.com/todos?userId=1&completed=true
11
4 et porro tempora
8 quo adipisci enim quam ut ab
10 illo est ratione doloremque quia maiores aut
```

- `params=parameters` adds `?userId=1&completed=true` to the URL. `response.url` shows the full URL that was used.
- The endpoint returns a JSON array, so `todos` is a **list** of dictionaries.
- `len(todos)` shows that user 1 has 11 completed tasks.
- `todos[:3]` takes the first three tasks, and the loop prints the ID and title of each.

## Sending Headers

Query parameters specify **which** data you want. But a request may also need to give the server extra information, such as which response formats the client can accept. This information is sent in **headers**, using the **`headers`** argument with a dictionary.

```python
import requests

url = "https://jsonplaceholder.typicode.com/todos/1"
request_headers = {"Accept": "application/json"}

response = requests.get(url, headers=request_headers)
print(response.status_code)
```

**Output:**

```
200
```

The `Accept` header tells the server which response format the client can accept. Here, the client indicates that it can accept JSON data. JSONPlaceholder returns JSON anyway, so this example shows **how to send** a header, not a visible change in the response. Each header is one key and value in the dictionary.

An API that expects its key in a header is called the same way. For example, if its documentation says to send `Authorization: Bearer YOUR_API_KEY`, the dictionary would be `{"Authorization": "Bearer " + api_key}`.

Some APIs require a key before they allow access to their data. Depending on the API, the key may need to be sent in a **header** or as a **query parameter**. Always follow the API's documentation.

## Making an Authenticated API Request

OpenWeatherMap expects the key in a query parameter named `appid`. Following **Web APIs and API Keys**, the key is read from an environment variable instead of being written in the code.

```python
import os
import requests

api_key = os.environ.get("WEATHER_API_KEY")

if not api_key:
    print("API key not set")
else:
    url = "https://api.openweathermap.org/data/2.5/weather"
    parameters = {"q": "Chennai", "appid": api_key}

    response = requests.get(url, params=parameters, timeout=10)
    print(response.status_code)
```

**Output (when the environment variable is not set):**

```
API key not set
```

- `os.environ.get("WEATHER_API_KEY")` reads your key from the environment variable.
- `if not api_key:` checks for the key **first**, so no request is sent when the key is missing.
- The key is added to the query parameters as `appid`, as the OpenWeatherMap documentation requires.
- `timeout=10` stops the program from waiting more than 10 seconds for a response.
- If the key is valid and activated, the API should return a successful response, typically with status code `200`. An invalid, missing or not-yet-activated key may result in an error such as **401**. The exact response depends on the API.

So far, every example has used GET requests to **retrieve** information. APIs can also **accept** data from programs. For example, a task-management app might let a program submit a new task. A **POST** request is commonly used for this.

## Sending Data with a POST Request

The `requests.post()` function sends a **POST** request, which is commonly used to submit data to a server, often to create something new. The data to send is passed with the **`json`** argument, and `requests` converts the Python dictionary into JSON for the request body.

```python
import requests

url = "https://jsonplaceholder.typicode.com/todos"
new_todo = {"userId": 1, "title": "Revise Python APIs", "completed": False}

response = requests.post(url, json=new_todo)

print(response.status_code)
print(response.json())
```

**Output:**

```
201
{'userId': 1, 'title': 'Revise Python APIs', 'completed': False, 'id': 201}
```

- The status code **201** means "Created".
- The response contains the task data and an `id` assigned by the practice API.
- JSONPlaceholder is a practice API, so it **pretends** to save the task but does not really store it. A real API would save it.

## Other Methods

POST is not the only method used to send changes to an API. Depending on the operation, you may need to replace existing data, update only part of it, or request that it be deleted. `requests` provides functions for these methods too:

| Function | Method | Usual Purpose |
| --- | --- | --- |
| `requests.get()` | GET | Read data |
| `requests.post()` | POST | Create new data |
| `requests.put()` | PUT | Replace existing data |
| `requests.patch()` | PATCH | Change part of existing data |
| `requests.delete()` | DELETE | Request deletion of data |

They all return a `Response` object, which is read in the same way.

## `params`, `headers` and `json` Compared

| Argument | What It Sends | Where It Goes | Example |
| --- | --- | --- | --- |
| `params` | Details that narrow down the data | The query string of the URL | `params={"userId": 1}` |
| `headers` | Extra information about the request | The request headers | `headers={"Accept": "application/json"}` |
| `json` | Data for the server to save or process | The request body | `json={"title": "Revise"}` |

## Points to Remember

- **`requests` is an external library.** Install it with `python -m pip install requests` inside a virtual environment.
- **`requests.get()`** sends a GET request and returns a `Response` object.
- **Check `response.status_code`** before using the data, and use **`response.json()`** to convert JSON into Python data.
- **Use `params`** for query parameters, **`headers`** for headers, and **`json`** to send data with POST.
- **Use a timeout in real programs** so a request does not wait forever for a response.

## Further Reading

- 📎 **Official `requests` quickstart guide** — https://requests.readthedocs.io/en/latest/user/quickstart/
- 📎 **JSONPlaceholder, the practice API used in this topic** — https://jsonplaceholder.typicode.com

Every example in this topic assumed the request would succeed. In real programs, the internet connection can fail, the server can be slow, or the data may not exist. In the next topic, **Handling API Errors**, you'll learn how to handle these problems safely.
