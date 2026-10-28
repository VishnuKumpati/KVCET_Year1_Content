# Handling API Errors

A program that calls an API depends on things it cannot control: the internet connection, the server and the data returned. The connection may fail, the server may respond too slowly, the requested data may not exist, or the response may not contain valid JSON.

These problems do not all behave the same way. **Some cause Python to raise an exception; others return a response that the program must check.**

> **Handling API errors means detecting problems that occur while calling an API and responding to them appropriately, instead of allowing unexpected failures to disrupt the program.**

This topic uses `try` and `except`, covered in **File Paths and Errors**, and the `requests` library and JSONPlaceholder API from **Calling an API with requests**.

## Two Kinds of API Problems

Problems with an API call fall into two groups. It is important to tell them apart, because Python handles them differently.

| Kind of Problem | What Happens | Examples | Does `requests` Raise an Exception? |
| --- | --- | --- | --- |
| **The request fails completely** | No response arrives from the server | No internet connection, server cannot be reached | **Yes**, automatically |
| **The server responds with an error** | A response arrives, but its status code shows a problem | 404 Not Found, 401 Unauthorized, 500 Internal Server Error | **No**, unless you ask for it |

The second row surprises many beginners. When the server answers with a **404**, `requests` does **not** raise an exception. It returns a `Response` object as usual, and the program continues. **You must check the status code yourself.**

## Checking the Status Code

The simplest way to detect a server error is to check `response.status_code` before using the data.

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/todos/999999", timeout=10)

print(response.status_code)
print(response.ok)

if response.status_code == 404:
    print("Task not found")
elif response.status_code == 200:
    print(response.json()["title"])
else:
    print("Unexpected status code:", response.status_code)
```

**Output:**

```
404
False
Task not found
```

- No task has the ID `999999`, so the server responds with **404**.
- `response.ok` is `False`, because 404 is not below 400.
- The three branches handle three different results:
  - **404:** the task does not exist, so a clear message is printed.
  - **200:** the request succeeded, so the title is printed.
  - **Anything else**, such as 401 or 500: the status code is printed, because the problem is something other than a missing task.

Without this check, `response.json()["title"]` would fail, because the response body is an empty `{}` with no `"title"` key.

Every example in this topic includes `timeout=10`, as recommended in **Calling an API with requests**.

## Turning Error Status Codes into Exceptions

Checking the status code with `if` works well when you want to handle a particular response yourself. But if you want Python to treat HTTP error responses as **exceptions**, `requests` provides a method called `raise_for_status()`.

A `try` block contains code that might raise an exception. If an exception occurs, Python **skips the remaining statements** in that block and looks for a matching `except` block. If one matches, its code runs.

> **`raise_for_status()` is a `Response` method that raises an `HTTPError` exception if the status code shows an error (400 or above). If the status code shows success, it does nothing.**

```python
import requests

try:
    response = requests.get("https://jsonplaceholder.typicode.com/todos/999999", timeout=10)
    response.raise_for_status()
    print(response.json())
except requests.exceptions.HTTPError:
    print("The server returned an error:", response.status_code)
```

**Output:**

```
The server returned an error: 404
```

1. `requests.get()` receives a response with status code **404**.
2. `response.raise_for_status()` sees that 404 is an error status, so it raises an **`HTTPError`**.
3. Python skips `print(response.json())` and jumps to the matching `except` block.
4. The `except` block prints a clear message.

If the status code had been `200`, `raise_for_status()` would do nothing, and the data would be printed.

`requests.exceptions.HTTPError` is the full name of the exception. All the exceptions in this topic are inside `requests.exceptions`.

`raise_for_status()` handles HTTP error responses, such as 404 and 500. But what happens if the program cannot reach the server and receives **no HTTP response at all**? In that case, `requests` raises a connection-related exception.

## Handling Connection Errors

If the program cannot reach the server at all, for example because there is no internet connection or the address is wrong, `requests` raises a **`ConnectionError`**.

```python
import requests

try:
    response = requests.get("https://this-site-does-not-exist.invalid", timeout=10)
except requests.exceptions.ConnectionError:
    print("Could not connect to the server")
```

**Output:**

```
Could not connect to the server
```

The address ends with `.invalid`, which is reserved for examples and never belongs to a real website. So no server can be reached, and `requests` raises a `ConnectionError`.

## Handling Timeouts

Sometimes the server is reachable, but the request takes too long. Instead of waiting forever, you can set a **timeout**.

`timeout=10` sets a limit on how long `requests` waits for a connection or for data to arrive. If that limit is exceeded, `requests` raises a **`Timeout`** exception.

```python
import requests

try:
    response = requests.get("https://jsonplaceholder.typicode.com/todos/1", timeout=0.001)
except requests.exceptions.Timeout:
    print("The server took too long to respond")
```

**Possible output:**

```
The server took too long to respond
```

This example uses an extremely short timeout, `0.001` seconds, to make a timeout **more likely** so you can see it happen. It will often raise a `Timeout` exception, but network behaviour can vary. In real programs, use a sensible timeout, such as `10` seconds.

So far, every problem has been about **receiving** a response. But receiving a response does not guarantee that its contents are in the format you expect. For example, a request may return HTML instead of JSON.

## Handling Invalid JSON

A response can arrive successfully but still not contain JSON. For example, a website's home page sends back HTML, the language of web pages. Calling `response.json()` on it raises a **`JSONDecodeError`**.

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com", timeout=10)

try:
    data = response.json()
except requests.exceptions.JSONDecodeError:
    print("The response is not valid JSON")
```

**Output:**

```
The response is not valid JSON
```

The base URL of JSONPlaceholder returns its home page, not API data. The status code is `200`, but the body is HTML, so it cannot be converted into Python data.

In a real program, check the HTTP status **and** whether the response contains valid JSON. A response can have a successful status code but contain data in an unexpected format.

## Common `requests` Exceptions

| Exception | When It Is Raised |
| --- | --- |
| `HTTPError` | `raise_for_status()` finds an error status code, such as 404 or 500 |
| `ConnectionError` | The server cannot be reached |
| `Timeout` | The server does not respond within the timeout |
| `JSONDecodeError` | `response.json()` is called on a body that is not valid JSON |
| `RequestException` | A general exception type that can catch many errors raised by `requests`, including the exceptions listed above |

If you do not need to tell different request failures apart, you can catch the general `RequestException` type. This is useful when you only need to show one message for any of these request problems:

```python
import requests

try:
    response = requests.get("https://this-site-does-not-exist.invalid", timeout=10)
    response.raise_for_status()
except requests.exceptions.RequestException:
    print("The request failed")
```

**Output:**

```
The request failed
```

## Putting It All Together

The example below is a **combined practice example**. It brings together every technique from this topic in one function. Each part has already been explained separately, so read it one block at a time.

```python
import requests

def get_todo(todo_id):
    url = "https://jsonplaceholder.typicode.com/todos/" + str(todo_id)

    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.HTTPError:
        print("Error: the server returned status code", response.status_code)
    except requests.exceptions.Timeout:
        print("Error: the server took too long to respond")
    except requests.exceptions.ConnectionError:
        print("Error: could not connect to the server")
    except requests.exceptions.JSONDecodeError:
        print("Error: the response is not valid JSON")

    return None

todo = get_todo(1)
if todo:
    print("Title:", todo["title"])

todo = get_todo(999999)
if todo:
    print("Title:", todo["title"])
```

**Output:**

```
Title: delectus aut autem
Error: the server returned status code 404
```

- **Task 1 exists.** The request succeeds, `raise_for_status()` does nothing, and the function returns the task's data.
- **Task 999999 does not exist.** `raise_for_status()` raises an `HTTPError`, the function prints a message and returns `None`.
- `if todo:` uses the data only when the function returned a task, because `None` counts as false.

When the request succeeds, the function returns the task data. When a handled error occurs, it prints a message and returns `None`, meaning no task data was returned. The `if todo:` check prevents the program from trying to read a title when no task was returned.

### The Order of `except` Blocks Matters

Python checks `except` blocks **from top to bottom** and runs the first matching block. Some exception types overlap. For example, `ConnectTimeout`, the exception raised when connecting takes too long, is both a `Timeout` and a `ConnectionError`.

Placing **`Timeout` before `ConnectionError`** gives connection timeouts the more specific timeout message. As a general rule, **place more specific exceptions before more general ones**, and put `RequestException` last if you include it.

## Points to Remember

- **A 404 or 500 response does not raise an exception by itself.** Check the status code, or call `raise_for_status()`.
- **`ConnectionError`** means the server could not be reached, and **`Timeout`** means it did not respond in time.
- **`JSONDecodeError`** means the response body is not valid JSON, even if the status code is 200.
- **Order `except` blocks from specific to general**, and always set a `timeout`.

## Further Reading

- 📎 **Official `requests` guide to errors and exceptions** — https://requests.readthedocs.io/en/latest/user/quickstart/#errors-and-exceptions
- 📎 **Official Python tutorial on handling exceptions** — https://docs.python.org/3/tutorial/errors.html#handling-exceptions

You can now call an API and handle the problems that may occur. In the next topic, **Building a Small API Program**, you'll combine everything from this week and the files topics into one complete program.
