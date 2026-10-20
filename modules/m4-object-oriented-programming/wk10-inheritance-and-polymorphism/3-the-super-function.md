# The `super()` Function

When a child overrides a method, calling that method on a child object runs the child's version, not the parent's version automatically. Sometimes, the child still needs the parent's work.

Without `super()`, the child might have to **copy the parent's code** into its own method. This creates duplication: if the parent's code changes, the copied code may need updating too.

The **`super()`** function lets a child **reuse the parent's method** instead of copying its code.

## The `super()` Function

>**`super()` allows a child class to call methods from its parent class, making it easier to reuse the parent's code without repeating it.**

`super().method_name()` calls the next version of `method_name()` in Python's inheritance lookup order. In these examples, that is the parent's version.

Although it is called the `super()` function, `super` is technically a built-in class. You use it in the same way as a function.

## Calling the Parent's `__init__` Method

The most common use of `super()` is inside the child's `__init__()` method. The child calls the parent's `__init__()` to set up the attributes the parent already handles, and then adds its own attributes.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, roll_number):
        super().__init__(name)
        self.roll_number = roll_number

student1 = Student("Ravi", 101)

student1.display_name()
print(student1.roll_number)
```

**Output:**

```
Name: Ravi
101
```

Here is what happens when `Student("Ravi", 101)` runs:

1. Python runs the `__init__()` method of `Student`.
2. The line `super().__init__(name)` calls the `__init__()` method of `Person`.
3. `Person` stores the name `"Ravi"` in the object.
4. `Student` then stores the roll number `101` in the object.

**The parent's `__init__()` sets up the name.** In this example, if `Student` does not call it or set `self.name` itself, `display_name()` raises an **`AttributeError`** because the object has no `name` attribute.

**Do not write `self` inside `super().__init__()`.** Python adds it automatically.

## Calling Other Parent Methods

`super()` works with **any method** of the parent class, not only `__init__()`. This lets a child class **extend** a parent's method: the child runs the parent's version first, and then adds extra work of its own.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_details(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, roll_number):
        super().__init__(name)
        self.roll_number = roll_number

    def display_details(self):
        super().display_details()
        print("Roll Number:", self.roll_number)

student1 = Student("Ravi", 101)
student1.display_details()
```

**Output:**

```
Name: Ravi
Roll Number: 101
```

Here, `super().display_details()` runs the parent's method to print the name. The child then prints the roll number.

## Replacing and Extending a Method

When a child overrides a method, it decides whether to reuse the parent's behaviour.

- **Replacing:** The child defines its own method without calling `super()`. Only the child's version runs.
- **Extending:** The child calls `super()` inside its method, reusing the parent's version and adding its own behaviour.

For example, `Student` can replace `display_details()` to print only the roll number, or extend it by calling `super().display_details()` to print both the name and the roll number.

## Calling the Parent Class Directly

A parent's method can also be called by writing the parent class name directly, such as `Person.__init__(self, name)`.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def display_name(self):
        print("Name:", self.name)

class Student(Person):
    def __init__(self, name, roll_number):
        Person.__init__(self, name)
        self.roll_number = roll_number

student1 = Student("Ravi", 101)

student1.display_name()
print(student1.roll_number)
```

**Output:**

```
Name: Ravi
101
```

This gives the same result, but it has two differences from `super()`:

| | `super().__init__(name)` | `Person.__init__(self, name)` |
| --- | --- | --- |
| Passing `self` | Done automatically | Must be written by hand |
| Parent class name | Not written | Written inside the child class |

`super()` avoids naming the parent class in the method call, making the code easier to maintain when the inheritance structure changes. It is generally preferred over direct parent-class calls.

## Points to Remember

- **Use `super()` without arguments inside an instance method or a class method.** Python uses the current class and the object or class associated with the method call to determine where method lookup should continue. It cannot be used on its own at the top level of a program.
- **The parentheses are required.** The correct form is `super().method_name()`, not `super.method_name()`.
- **When a class has more than one parent class**, `super()` follows the class's inheritance lookup order. This is covered in **Types of Inheritance**.

## Further Reading

- 📎 **Official Python reference for `super()`** — https://docs.python.org/3/library/functions.html#super

The examples in this topic use **single inheritance**, where a class has one parent class. In the next topic, **Types of Inheritance**, you'll explore other inheritance structures, including multiple inheritance.
