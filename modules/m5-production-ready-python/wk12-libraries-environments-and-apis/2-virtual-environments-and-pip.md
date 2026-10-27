# Virtual Environments and pip

External libraries must be installed before they can be used. By default, Python installs every library into **one shared place** on the computer. This works for a single small project, but it causes two problems as you work on more projects.

**Projects may need different versions of the same library.** One project may need version 1 of a library, while another needs version 2. Only one version can be installed in the shared place. Installing it for the second project can break the first project, often without any warning.

**It becomes unclear what each project needs.** If many libraries are installed in one place, it is hard to tell which ones a particular project actually uses. Sharing that project with someone else then becomes guesswork.

Python solves both problems with **virtual environments**.

> **A virtual environment is a separate folder for one project, with its own Python setup and its own installed libraries. It keeps each project's libraries isolated, so projects do not interfere with each other.**

## Before You Start

The commands in this topic are typed into a **terminal**, not into a Python program: Command Prompt or PowerShell on Windows, or Terminal on macOS and Linux.

To check that Python is installed, run:

```
python --version
```

If this does not work, try `py --version` on Windows, or `python3 --version` on macOS and Linux. Use whichever command works wherever this topic shows `python`.

## Creating a Virtual Environment

Open the terminal inside your project folder and run:

```
python -m venv venv
```

- `python -m venv` runs the **`venv`** module from the standard library, which creates virtual environments.
- The second `venv` is the **name of the folder** to create. `venv` and `.venv` are the most common names.

Your project folder now contains a `venv` folder. It holds the environment's settings, the Python it uses and its own installed libraries.

## Activating the Virtual Environment

Creating an environment does not start using it. You must **activate** it, so that `python` and `pip` use this environment instead of the shared one.

| Terminal | Command |
| --- | --- |
| Windows Command Prompt | `venv\Scripts\activate.bat` |
| Windows PowerShell | `.\venv\Scripts\Activate.ps1` |
| macOS or Linux | `source venv/bin/activate` |

When the environment is active, its name appears at the start of the terminal line:

```
(venv) C:\Users\Ravi\project>
```

**If `(venv)` is not shown, the environment is not active**, and anything you install goes to the shared place instead.

- **Activation lasts only for that terminal window.** A new terminal starts without it, so activate the environment again each time.
- If PowerShell blocks the activation command, use Command Prompt instead. Do not change your computer's security settings unless you understand what the change does.

To stop using the environment, run `deactivate`. The `(venv)` label disappears.

## Checking the Active Environment

A new environment contains almost nothing. Run:

```
python -m pip list
```

**Possible output:**

```
Package Version
------- -------
pip     24.0
```

Only `pip` itself is listed. Libraries installed in the shared place are not visible here, which shows that the environment is isolated. Your `pip` version may be different.

## Installing a Library

`pip` downloads a library from the **Python Package Index (PyPI)** and installs it in the active environment. Downloading needs an internet connection.

```
python -m pip install requests
```

**`python -m pip`** runs `pip` using the same Python as the active environment, so the library is installed for the Python that will run your program. This topic uses this form for every `pip` command.

Many libraries depend on other libraries to work. These are called **dependencies**, and `pip` installs them automatically. For example, installing `requests` also installs four other libraries that `requests` needs:

```
python -m pip list
```

**Possible output:**

```
Package            Version
------------------ ---------
certifi            2026.10.19
charset-normalizer 3.5.2
idna               3.20
pip                24.0
requests           2.34.2
urllib3            2.8.0
```

You asked for one library, and five were installed. The version numbers on your computer may be different.

## Common pip Commands

| Command | What It Does |
| --- | --- |
| `python -m pip install requests` | Installs the latest compatible version |
| `python -m pip install requests==2.31.0` | Installs a specific version |
| `python -m pip install --upgrade requests` | Upgrades to the latest version |
| `python -m pip uninstall requests` | Removes a library |
| `python -m pip list` | Lists all installed libraries |
| `python -m pip show requests` | Shows details about one library |
| `python -m pip freeze` | Lists installed libraries with their exact versions |

### Checking a Library with `pip show`

```
python -m pip show requests
```

**Possible output (shortened):**

```
Name: requests
Version: 2.34.2
Summary: Python HTTP for Humans.
Requires: certifi, charset_normalizer, idna, urllib3
Required-by:
```

- **`Requires`** lists the libraries that `requests` depends on.
- **`Required-by`** lists the installed libraries that depend on `requests`. Here it is empty, so no other installed library needs it.

`python -m pip uninstall requests` removes **only** `requests`. Its four dependencies stay installed, because `pip` cannot be sure whether something else still needs them.

## Saving Libraries in `requirements.txt`

A project should record the libraries it needs, so the same setup can be created again. The common way is a file named **`requirements.txt`**.

> **`requirements.txt` is a text file that lists the libraries a project needs, so they can be installed again with one command.**

To create it, run:

```
python -m pip freeze > requirements.txt
```

`>` saves the output of `pip freeze` into the file instead of showing it on the screen. `pip freeze` lists every library installed in the active environment, including dependencies.

The file looks similar to this, with the versions installed on your computer:

```
certifi==2026.10.19
charset-normalizer==3.5.2
idna==3.20
requests==2.34.2
urllib3==2.8.0
```

To install every library from the file into another environment, run:

```
python -m pip install -r requirements.txt
```

**After installing a new library, run `pip freeze` again** to update `requirements.txt`. This step is easy to forget.

## Pinning Versions

`pip freeze` writes `requests==2.34.2` rather than just `requests`. The `==` **pins** the library to that exact version.

- **Without a pin**, installing from `requirements.txt` gives the newest version available that day. If a newer version behaves differently, the project can break even though its code has not changed.
- **With a pin**, everyone gets the same versions, and the project keeps working.

Pinned versions do not update themselves, so they can miss later fixes, including security fixes. Upgrade them deliberately: change the version, test the project, then update `requirements.txt`.

## Sharing a Project

When you share a project:

- **Share** your code and the `requirements.txt` file.
- **Do not share** the `venv` folder. It is large, works only on your computer, and can be recreated from `requirements.txt`.
- If you use **Git**, add `venv/` to your `.gitignore` file so the folder is not saved in your repository.

The other person creates their own environment, activates it and runs `python -m pip install -r requirements.txt`. They should use a compatible Python version.

## The Everyday Routine

| Situation | Steps |
| --- | --- |
| **Starting a new project** | Create the environment, activate it, install libraries, then save `requirements.txt` |
| **Returning to a project** | Open a terminal in the project folder and activate the environment |
| **Setting up someone else's project** | Create a new environment, activate it, then run `python -m pip install -r requirements.txt` |

## Fixing Common Problems

| Problem | What to Try |
| --- | --- |
| `python` is not recognised | Try `py` on Windows or `python3` on macOS and Linux, and check that Python is installed. |
| `pip` is not found | Use `python -m pip` after activating the environment. |
| A library installs but cannot be imported | Check that `(venv)` is shown, and that the library was installed while the environment was active. |
| It worked yesterday, but not today | The new terminal is probably not activated. Activate the environment again. |

## Points to Remember

- **Create one virtual environment for each project** using `python -m venv venv`, and **activate it** in every new terminal.
- **Use `python -m pip`** so libraries are installed for the active environment's Python.
- **`pip` installs dependencies automatically**, and `requirements.txt` records every library a project needs.
- **Share `requirements.txt`, not the `venv` folder.**

## Further Reading

- 📎 **Official Python tutorial on virtual environments and packages** — https://docs.python.org/3/tutorial/venv.html
- 📎 **Official pip user guide** — https://pip.pypa.io/en/stable/user_guide/

Every library you install comes with its own functions and rules. In the next topic, **Reading Documentation**, you'll learn how to find out how a library works by reading its official documentation.
