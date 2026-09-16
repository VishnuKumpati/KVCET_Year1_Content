# Virtual Environments and pip

The standard library covers a great deal and not everything. Talking to a web service, analysing a table of data or running a test suite means installing code someone else wrote.

Installing it carelessly breaks things. This chapter is about doing it properly.

## The Problem

A single Python installation has one place where packages live. Install everything there and two problems follow.

**Projects disagree about versions.** One project needs version 1 of a library, another needs version 2. Only one can be installed, so installing for the second breaks the first — and nothing announces it. The failure arrives later, as an error in code that was working.

**Nobody knows what a project needs.** With fifty packages installed globally, a project that imports three of them gives no indication which three. Handing it to someone else, or to a server, becomes guesswork.

## Virtual Environments

A **virtual environment** is a directory containing its own Python and its own installed packages. Each project gets one, and they cannot interfere with each other.

Create one with the `venv` module, from the project directory:

```
python -m venv venv
```

The first `venv` is the module; the second is the directory to create. Naming the directory `venv` or `.venv` is conventional.

That creates:

```
venv/
    Scripts/        (Windows)  or  bin/  (macOS and Linux)
    Lib/            (Windows)  or  lib/  (macOS and Linux)
    pyvenv.cfg
```

## Activating

Creating an environment does not start using it. **Activating** puts its Python first on the path for the current terminal session.

| Shell | Command |
| --- | --- |
| Windows PowerShell | `.\venv\Scripts\Activate.ps1` |
| Windows Command Prompt | `venv\Scripts\activate.bat` |
| macOS and Linux | `source venv/bin/activate` |

The prompt changes to show the active environment:

```
(venv) C:\Users\anita\project>
```

That prefix is the signal that `python` and `pip` now refer to the environment rather than the system installation. **If it is not there, the environment is not active**, and anything installed goes to the wrong place.

`deactivate` — the same word on every platform — returns to normal.

Activation applies to one terminal session. A new terminal starts unactivated, which is the single commonest cause of "it worked yesterday".

On Windows PowerShell, activation may be refused by the execution policy. `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` permits it.

## Checking

A fresh environment contains almost nothing:

```
pip list
```

**Output:**

```
Package Version
------- -------
pip     25.3
```

Just `pip` itself. Nothing from the system installation is visible — that is the isolation working.

## Installing

`pip install` fetches a package and its dependencies from PyPI, the Python Package Index:

```
pip install requests
```

**Output:**

```
Collecting requests
  Using cached requests-2.34.2-py3-none-any.whl.metadata (4.8 kB)
Collecting charset_normalizer<4,>=2 (from requests)
Collecting idna<4,>=2.5 (from requests)
Collecting urllib3<3,>=1.26 (from requests)
Collecting certifi>=2023.5.7 (from requests)
Installing collected packages: urllib3, idna, charset_normalizer, certifi, requests

Successfully installed certifi-2026.7.22 charset_normalizer-3.5.1 idna-3.19 requests-2.34.2 urllib3-2.8.0
```

One package was asked for and five were installed. `requests` depends on four others, and pip worked that out and fetched them. This is why manual installation is not a realistic alternative.

```
pip list
```

**Output:**

```
Package            Version
------------------ ---------
certifi            2026.7.22
charset-normalizer 3.5.1
idna               3.19
pip                25.3
requests           2.34.2
urllib3            2.8.0
```

`python -m pip install requests` does the same thing and is worth preferring. It guarantees the `pip` belonging to the `python` you are running, which removes a whole category of confusion about where a package went.

## The Other pip Commands

| Command | Does |
| --- | --- |
| `pip install name` | install the latest version |
| `pip install name==2.34.2` | install exactly that version |
| `pip install --upgrade name` | upgrade to the latest |
| `pip uninstall name` | remove it |
| `pip list` | list what is installed |
| `pip show name` | details about one package |
| `pip freeze` | list installed packages in a pinned format |

`pip show` reports where a package came from and what it needs:

```
pip show requests
```

**Output:**

```
Name: requests
Version: 2.34.2
Summary: Python HTTP for Humans.
Requires: certifi, charset_normalizer, idna, urllib3
Required-by:
```

`Requires` is what it depends on; `Required-by` is what depends on it. An empty `Required-by` means nothing else needs it, so it is safe to uninstall.

Note that `pip uninstall requests` removes only `requests`. Its four dependencies stay, because pip cannot know whether something else wants them.

## requirements.txt

A project should record what it depends on. `pip freeze` produces exactly that list:

```
pip freeze
```

**Output:**

```
certifi==2026.7.22
charset-normalizer==3.5.1
idna==3.19
requests==2.34.2
urllib3==2.8.0
```

Save it to the conventional filename:

```
pip freeze > requirements.txt
```

Anyone else — or a server, or you on another machine — then recreates the environment exactly:

```
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

`requirements.txt` goes into version control. The `venv` directory does not.

## Pinning

`pip freeze` writes `requests==2.34.2` rather than just `requests`. The `==` **pins** the version, and that is deliberate.

Without a pin, `pip install -r requirements.txt` installs whatever is newest today. A library that changes behaviour in a later release then breaks a project that has not been touched in months, and the cause is invisible — the code did not change.

With pins, everyone gets the same versions and the project keeps working until someone deliberately upgrades.

The trade-off is that pinned versions grow old, including their security fixes. Upgrade deliberately: change the pin, run the tests, commit.

Some projects keep two files — direct dependencies with loose constraints for humans, and a fully pinned file generated from it. That is worth knowing about and beyond what a first project needs.

## Never Commit the Environment

Add this to `.gitignore`:

```
venv/
.venv/
__pycache__/
*.pyc
```

A virtual environment is large, contains compiled files specific to one operating system, and is fully reproducible from `requirements.txt`. Committing it is a common beginner mistake that makes a repository enormous and unusable on other platforms.

`__pycache__` directories are Python's compiled bytecode caches — regenerated automatically, and equally not worth committing.

## The Routine

Starting a project:

```
mkdir project
cd project
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install requests
pip freeze > requirements.txt
```

Returning to it later:

```
cd project
.\venv\Scripts\Activate.ps1
```

Joining someone else's:

```
git clone <url>
cd project
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

After installing anything new, regenerate `requirements.txt` and commit it. That is the step people forget, and it is what makes a project work for the next person.

## Further Reading

- **Official guide to virtual environments and packages** — https://docs.python.org/3/tutorial/venv.html
- **Python virtual environments explained** — https://realpython.com/python-virtual-environments-a-primer/

One environment per project, activated before working, with `requirements.txt` in version control and the environment directory out of it. Next, finding out how an installed package actually works.
