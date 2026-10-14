# Introduction to Object-Oriented Programming

Imagine building a college application that manages hundreds of students.

Each student has details such as a name and marks. The application must also perform actions such as adding bonus marks and checking whether a student has passed.

As the application grows, keeping student data and all the operations related to it organised becomes important.

> **Object-oriented programming (OOP) is a way of organising a program around objects that hold data and provide methods to work with that data.**

Instead of treating data and the operations on it as completely separate parts, OOP brings related data and behaviour together. This makes it easier to represent things from the real world in a program, such as students, bank accounts, or library books.

For example, a student in a college application has data such as a name and score, along with operations such as adding bonus marks or checking the result. OOP provides a way to organise these related parts together.

## The Terms Used in Object-Oriented Programming

Four terms appear throughout this week.

| Term | Meaning |
| --- | --- |
| Class | A definition of the data and behaviour that objects of that type can have. |
| Object | An individual thing created from a class. |
| Attribute | A piece of data belonging to an object, such as a name or a score. |
| Method | A function belonging to an object, such as adding bonus marks. |

In the college application, `Student` would be the class, and each individual student would be an object created from it. A name and a score are attributes. Adding bonus marks and checking the result are methods.

Each of these terms has a topic of its own in the lessons that follow.

## Objects You Have Already Used

Objects are not new to Python programs. The values you have used so far, such as strings, lists and dictionaries, are objects of built-in types.

```python
print(type("hello"))
print(type([1, 2]))
print(type({"a": 1}))
```

**Output:**

```
<class 'str'>
<class 'list'>
<class 'dict'>
```

Each output names the class the value belongs to.

This also explains the methods used throughout the course. `"hello".upper()` and `marks.append(95)` are methods belonging to objects of the `str` and `list` classes.

The difference now is that you will write your own classes rather than only using Python's.

## Further Reading

- 📎 **Object-oriented programming with worked examples** — https://www.programiz.com/python-programming/object-oriented-programming
- 📎 **Official Python guide to classes** — https://docs.python.org/3/tutorial/classes.html

You will explore how this works in the following topics, starting with the two fundamental concepts behind OOP: classes and objects.
