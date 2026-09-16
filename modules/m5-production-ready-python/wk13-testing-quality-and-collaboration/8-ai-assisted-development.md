# AI-Assisted Development

AI coding assistants — Copilot, Claude, ChatGPT, Cursor and others — generate code from a description, explain unfamiliar code, and suggest completions as you type. They are now a normal part of professional development.

They are also confidently wrong on a regular basis, and the responsibility for what ships is entirely yours. This chapter is about using them without being misled by them.

## What They Are Good At

**Boilerplate.** An `argparse` setup, a class with ten attributes, a `setup.py`. Code that is tedious rather than difficult, where you would recognise a mistake instantly.

**Unfamiliar syntax.** "How do I read a CSV with a semicolon delimiter?" — faster than searching, and usually right for something this common.

**Explaining code.** Pasting in a dense regular expression or an unfamiliar function and asking what it does is one of the strongest uses, because you can verify the answer against the code in front of you.

**First drafts of tests.** They are good at enumerating edge cases, which is exactly the thing beginners find hard. Review each one — some will test the wrong thing — but the list is a genuine starting point.

**Rubber-ducking.** Describing a bug often reveals it, and an assistant asks useful follow-up questions.

## What They Are Bad At

**Inventing things that do not exist.** This is the failure to understand first. Models generate plausible text, and a plausible method name is not a real one:

```python
print("hello".titlecase())
```

**Output:**

```
AttributeError: 'str' object has no attribute 'titlecase'
```

```python
import json
print(json.parse("{}"))
```

**Output:**

```
AttributeError: module 'json' has no attribute 'parse'
```

`titlecase()`, `json.parse()`, `list.sort_by()`, `datetime.add_days()` — all reasonable-sounding, all non-existent. Other languages have them, so they appear in the training data and get blended into Python.

These are the **safe** failures: the code does not run, and the error names the problem.

**Code that runs and is subtly wrong.** The dangerous failure. An off-by-one in a slice, `>` where `>=` was needed, a currency calculation using floats, an average that ignores the empty case. It runs, it produces numbers, and the numbers are wrong.

**Out-of-date patterns.** Training data is full of old code. Expect `os.path` instead of `pathlib`, `%` formatting instead of f-strings, and libraries whose APIs changed two versions ago.

**Security.** Generated code frequently contains SQL built by string concatenation, missing input validation, hard-coded credentials, disabled certificate verification, or `eval()` on untrusted input. It reproduces the insecure patterns that are common in its training data.

**Your specific context.** It does not know your project's conventions, which module a function belongs in, or that your team decided against a library six months ago.

## The Obligation

**You are responsible for every line you commit, regardless of who or what wrote it.**

"The AI generated it" is not a defence in code review, in an incident report, or in an assessment. The commit carries your name.

So a practical standard for generated code:

**Read every line.** If you cannot explain what a line does, it is not ready to commit. This single rule prevents most of the damage.

**Run it.** Including the edge cases from earlier in this week — empty input, zero, the boundary values.

**Check anything it claims exists.** `help()` on the function, or the library's documentation. Ten seconds, and it catches hallucinated APIs outright.

**Test it.** Write the tests yourself, or at minimum review generated tests individually. Tests written by the same model that wrote the bug tend to share the bug's assumptions.

**Watch the imports.** Generated code sometimes imports a package that does not exist, or one that does exist and is not what was meant. A package installed on a hallucinated name is a real supply-chain risk.

## Never Paste Secrets

Anything sent to a hosted assistant leaves your machine, and may be retained or used for training depending on the service and its settings.

Never paste:

- API keys, passwords, tokens, connection strings
- customer data, personal data, anything covered by privacy law
- proprietary code your employer or institution has not permitted you to share
- security details of a system you operate

Replace real values with placeholders before asking. Check what your organisation's policy actually permits — many have one, and many people have not read it.

## Licensing and Attribution

Models are trained on public code, including code under licences with conditions. A generated snippet may closely reproduce licensed source, and the legal position is still unsettled in most jurisdictions.

For coursework and small projects the practical risk is low. It becomes real in commercial work, and the rule there is to follow your employer's policy rather than your own judgement.

Be honest about what you used. Where a course, an employer or a competition asks, say which parts were AI-assisted. Where a project's conventions call for it, note significant generated contributions. Concealing it is the problem, not the use.

## The Real Risk While Learning

This is the part that matters most in a first year, and it is not a legal or a technical risk.

**Reading generated code feels like understanding it.** It does not build the same knowledge as writing it. A student who generates every exercise passes the exercises and cannot debug anything, because debugging requires a model of how the code works — and that model is built by writing code, getting it wrong, and fixing it.

The difficulty you are avoiding is the learning.

A reasonable practice while studying:

**Attempt it first.** Write your own version before asking. Even a broken attempt means you have thought about the problem, and you will read the answer critically rather than accepting it.

**Ask for explanations more than solutions.** "Why does this raise `IndexError`?" teaches; "write this function for me" does not.

**Type it out rather than pasting.** Slower, and it forces you to read every line.

**Close the assistant sometimes.** Deliberately solve some problems unaided. Interviews, exams and outages all happen without it.

**Never submit code you cannot explain.** Assessments routinely include a conversation about your submission, and this is also simply the professional standard.

Check your institution's rules on AI assistance. They vary by course and by assignment, they are usually specific, and "I did not know the policy" has never worked.

## A Working Practice

A reasonable default once you are competent:

- Use it freely for **boilerplate and syntax lookups** — low risk, high saving.
- Use it for **explanation** — high value, and verifiable against the code in front of you.
- Use it for **first drafts** of tests and documentation, then review every line.
- Be cautious with **business logic** — the place subtle errors are most expensive.
- Be very cautious with **security-sensitive code** — authentication, cryptography, anything touching a database or user input.
- Do not use it for **things you do not understand well enough to review**. That is the line, and it moves as you learn.

The tool amplifies whatever judgement you bring. An experienced developer gets a fast assistant; an inexperienced one gets confident wrong answers they cannot evaluate. Building the judgement is what this year is for, and the tool does not replace it.

## Further Reading

- **GitHub Copilot documentation** — https://docs.github.com/en/copilot
- **OWASP Top Ten security risks** — https://owasp.org/www-project-top-ten/

Use AI assistance for speed, verify everything it produces, never send it secrets, be honest about using it, and do not let it stand in for the practice that builds your own judgement.

That completes this module. A project now has files it can read and write, dependencies it declares, an API it can call, tests that check it, a style anyone can read, logs that explain what it did, and a history that records how it got there — which is the difference between code that runs and code that can be handed to someone else.
