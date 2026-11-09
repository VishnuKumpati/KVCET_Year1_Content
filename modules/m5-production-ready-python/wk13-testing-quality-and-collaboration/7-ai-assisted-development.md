# AI-Assisted Development

AI tools are now part of everyday programming. They can explain an error message in seconds, suggest the next line of code, or write a first draft of a function from a short description. Used well, they save time and help programmers learn. Used carelessly, they produce code that looks correct but contains mistakes the programmer does not notice.

The skills from this week, especially testing, edge cases, logging and Git, help you check AI-generated code and use AI tools more responsibly.

> **AI-assisted development is the practice of using artificial intelligence tools to help write, explain, test and review code, while the programmer remains responsible for checking and understanding the result.**

## Key Terms

| Term | Meaning |
| --- | --- |
| **AI coding assistant** | A tool that uses artificial intelligence to suggest, write or explain code |
| **Large language model (LLM)** | A type of AI trained on large amounts of text and code. It generates responses by predicting likely sequences of words and code based on patterns learned during training |
| **Prompt** | The instruction or question you give to an AI tool |
| **Hallucination** | A response from an AI tool that sounds correct and confident, but is wrong or invented |

Because an LLM **predicts** likely text rather than checking facts, it can hallucinate. This is the most important thing to understand about AI tools.

## Types of AI Coding Tools

| Type | How It Works | Examples |
| --- | --- | --- |
| **Chat assistants** | You type a question or request, and the tool replies in a conversation | Claude, ChatGPT, Gemini |
| **Code completion tools** | The tool runs inside your code editor and suggests code as you type | GitHub Copilot |

These categories can overlap. For example, GitHub Copilot can suggest code as you type and answer questions through its Chat view.

## How AI Helps During Development

Programmers use AI tools at every stage of writing a program. The table shows the main ways, with an example prompt for each.

| Stage | How AI Helps | Example Prompt |
| --- | --- | --- |
| **Planning** | Suggests how to break a task into steps or functions | "How should I structure a Python program that reads marks from a CSV file and prints each student's result?" |
| **Writing code** | Produces a first draft of a small function from a clear description | "Write a Python function that returns the average of a list of numbers." |
| **Understanding code** | Explains unfamiliar code line by line | "Explain what each line of this function does." |
| **Debugging** | Explains an error message and suggests possible causes | "What does this `KeyError` mean in my code, and how can I fix it?" |
| **Testing** | Suggests pytest tests, including edge cases | "Write pytest tests for this function, including boundary values and an empty list." |
| **Reviewing** | Points out possible bugs, unclear names or missing error handling | "Review this function for bugs and edge cases I may have missed." |
| **Documenting** | Writes docstrings and comments | "Write a docstring for this function." |
| **Learning** | Explains a concept in simple terms, with an example | "Explain the difference between a list and a tuple, with an example." |

In each case, AI **speeds up** the work, but you still check the result. The rest of this topic shows how.

## Using AI Inside Your Code Editor

Chat assistants are commonly accessed through a browser or app. Code completion tools work **inside your code editor**, where they use your code as context and suggest code as you type.

> **An IDE (Integrated Development Environment) is software that combines the tools needed to write, run, test and debug programs in one place.**

VS Code is a **code editor**. With extensions, it works like a full IDE, and it is the editor used throughout this course. This section shows how to add **GitHub Copilot**, a popular AI coding assistant for VS Code. Many other AI tools, such as Claude, also provide VS Code extensions that work in a similar way.

### Setting Up GitHub Copilot in VS Code

1. **Update VS Code** to the latest version, using **Help → Check for Updates** on Windows and Linux, or **Code → Check for Updates** on macOS.
2. **Hover over the Copilot icon** in the **Status Bar**, the bar along the bottom of VS Code.
3. **Select Use AI Features.**
4. **Choose a sign-in method** and follow the instructions to sign in with your GitHub account, the same account from **Git Fundamentals**.
5. If your account does not have a Copilot subscription, you can sign up for the **Copilot Free** plan. It provides limited access to inline suggestions and chat features. The monthly limits may change, so check the official documentation for current details.

Eligible students can also access additional Copilot features through the **GitHub Education** programme.

The VS Code interface changes over time. If these labels look different, follow the **Official VS Code guide to GitHub Copilot** in **Further Reading**.

### Inline Suggestions

Once Copilot is set up, it suggests code **as you type**. A suggestion appears as faded grey text, called **ghost text**, after your cursor.

For example, type this comment and the start of a function in a Python file:

```
# Return True if marks are from 0 to 100
def is_valid_marks(marks):
```

Copilot may suggest the next line in grey. Then:

- Press **Tab** to **accept** the suggestion.
- Press **Esc** to **reject** it, or simply keep typing.

The suggestion is based on your comment, the function name and the rest of the file. A **clear comment and a descriptive function name** lead to better suggestions, just as a clear prompt does.

### The Chat View

Copilot also has a **Chat view**, a panel where you can type questions, just as in a chat assistant. Open it from the **Chat** menu in the VS Code title bar and select **Open Chat**. You can also use **Ctrl + Alt + I** on Windows and Linux.

Because the Chat view runs inside VS Code, it can use your code as context:

1. **Select** some code in the editor.
2. **Open the Chat view** and ask a question, such as "Explain this code" or "Write pytest tests for this function."
3. **Read the response**. If it suggests code, review it before adding it to your file.

You can also ask about the whole file or an error shown in the terminal.

### Suggestions Still Need Checking

Code suggested in the editor is just as likely to contain mistakes as code from a chat assistant. **Do not press Tab without reading the suggestion.** Every suggestion you accept becomes your code, so it must pass the same checks described in **Checking AI-Generated Code**.

## The Limits of AI Tools

| Limit | What It Means |
| --- | --- |
| **It can be wrong** | Code may contain bugs, even when the explanation sounds confident |
| **It can invent things** | It may suggest a function, option or library that does not exist |
| **It can be out of date** | It may suggest code for an older version of Python or of a library |
| **It may lack project context** | It uses the code and other information made available to it, but may not understand every part of your project or how different files depend on each other |
| **It can introduce security problems** | Code may handle passwords, keys or user input unsafely |

**The programmer is always responsible for the code**, no matter who or what wrote it.

## Writing Effective Prompts

The quality of a response depends heavily on the prompt. An effective prompt is **specific** and gives **context**.

> **Prompt engineering is the practice of writing clear and specific instructions so that an AI tool produces more relevant and useful responses.**

Compare these two prompts:

**A vague prompt:**

```
write a marks function
```

The tool must guess what the function should do, what its inputs are and what counts as valid.

**A specific prompt:**

```
Write a Python function named is_valid_marks(marks) that returns True
if marks is from 0 to 100, including 0 and 100, and False otherwise.
Use Python 3. Explain each line of the function.
```

This prompt states:

- **The language and version**, Python 3.
- **The function's name and input**, `is_valid_marks(marks)`.
- **The exact rule**, including the boundary values `0` and `100`.
- **What to return**, `True` or `False`.
- **What else you want**, an explanation of each line.

### Elements of an Effective Prompt

1. **State the task clearly**, such as "Write a function" or "Explain this error".
2. **Give context**: the language, the version and any code or error message involved.
3. **State the requirements**, including edge cases and expected results.
4. **Ask for an explanation**, so you can understand the code and not just copy it.
5. **Refine and ask again** if the first response is not right, by explaining what is wrong.

## Checking AI-Generated Code

Never use AI-generated code without checking it. Suppose an AI tool returns this function for the specific prompt above:

**File: `marks.py`**

```python
def is_valid_marks(marks):
    return marks > 0 and marks <= 100
```

It looks correct, and it works for typical marks such as `50`. To check it, write pytest tests that include the **boundary values** from **Edge Cases**:

**File: `test_marks.py`**

```python
import pytest
from marks import is_valid_marks


@pytest.mark.parametrize("marks, expected", [
    (-1, False),
    (0, True),
    (50, True),
    (100, True),
    (101, False),
])
def test_is_valid_marks(marks, expected):
    assert is_valid_marks(marks) == expected
```

Run `python -m pytest`:

**Possible output (shortened):**

```
test_marks.py .F...                                                      [100%]
...
>       assert is_valid_marks(marks) == expected
E       assert False == True
E        +  where False = is_valid_marks(0)
...
FAILED test_marks.py::test_is_valid_marks[0-True] - assert False == True
========================= 1 failed, 4 passed in 0.02s ==========================
```

The test with `0` fails. The prompt said **0 is valid**, but the AI-generated code uses `marks > 0`, which rejects `0`. The correct condition is `marks >= 0`:

```python
def is_valid_marks(marks):
    return marks >= 0 and marks <= 100
```

After this fix, all five tests pass. A typical input such as `50` would never have found this mistake. **Tests found a bug in code that looked correct.**

### A Checklist for AI-Generated Code

Before using any code from an AI tool:

1. **Read every line** and make sure you understand what it does. If you do not, ask the tool to explain it.
2. **Run the code** to check that it works at all.
3. **Test it with pytest**, including edge cases such as boundary values, empty values and invalid input.
4. **Check the documentation** for any function or library it uses, to confirm that it exists and works as described.
5. **Check for security problems**, such as API keys written in the code, as warned in **Web APIs and API Keys**.
6. **Commit it with Git** only after it passes your checks, so you can return to the earlier version if a problem appears later.

## Using AI to Understand Errors

AI tools are especially useful for explaining error messages. Give the tool **both** the error message and the code that caused it:

```
I am learning Python. This code:

numbers = []
print(sum(numbers) / len(numbers))

gives this error:

ZeroDivisionError: division by zero

What does the error mean, why does it happen here, and how can I fix it?
```

The tool can explain that `len(numbers)` is `0` for an empty list, so the code divides by zero. Then **check the explanation yourself**, for example by testing an empty list, as in **Edge Cases**.

Asking for an **explanation**, not only a fix, helps you learn and recognise the same error next time.

## Using AI Responsibly

- **Protect sensitive information.** Never paste API keys, passwords or other secrets into an AI tool. Do not share personal data or private company code unless your organisation explicitly permits it.
- **Follow your college's or company's rules** about when AI tools may be used.
- **Do not submit code you cannot explain.** If someone asks how your code works, you should be able to answer.
- **Use AI to learn, not only to finish tasks.** Ask why the code works, try writing it yourself first, then compare.

## Points to Remember

- **AI-assisted development** means using AI tools to help with code, while you stay responsible for checking and understanding it.
- **AI tools can hallucinate**: they can produce confident answers that are wrong or invented.
- **Effective prompts are specific**: they give the task, the context, the requirements and ask for an explanation.
- **Always check AI-generated code** by reading it, running it, testing it with pytest and checking the documentation.

## Further Reading

- 📎 **Anthropic's guide to prompt engineering** — https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview
- 📎 **GitHub Copilot documentation: What is GitHub Copilot** — https://docs.github.com/en/copilot/get-started/what-is-github-copilot
- 📎 **Official VS Code guide to GitHub Copilot** — https://code.visualstudio.com/docs/copilot/overview

This completes the teaching content. You can now write Python programs, organise them with classes and modules, work with files and APIs, test and log your code, and collaborate with others using Git and GitHub. Most importantly, you have the skills to check that your code, whoever writes it, works correctly. Next, you will put all of these skills together in the **Capstone Project**.
