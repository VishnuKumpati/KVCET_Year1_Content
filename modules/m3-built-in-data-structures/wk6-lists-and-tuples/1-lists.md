# Lists

So far, each variable has held one value at a time. An `int` can hold one whole number, and a `str` can hold one piece of text. A list lets one variable hold many values together.

> **A list is an ordered collection of values stored in a single variable.** Each value in a list is called an element. A list can hold any number of elements, and those elements may be of any data type.

## Creating a List

A list is written with square brackets, and its elements are separated by commas.

**Example**

```python
marks = [87, 72, 95]
print(marks)
```

**Output:**

```
[87, 72, 95]
```

One variable now holds three values. The output shows the values inside square brackets, separated by commas.

## Finding the Length of a List

Once a list exists, the next thing a program usually needs is how many elements it holds. The `len()` function returns that number.

**Syntax**

```
len(list_name)
```

**Example**

```python
marks = [87, 72, 95]
print(len(marks))
```

**Output:**

```
3
```

`len()` is a built-in function, like `print()` and `int()`.

## Lists of Strings

The examples so far have used numbers, but lists can also hold strings. Strings go inside the brackets in the same way.

**Example**

```python
names = ["Asha", "Ravi", "Meera"]
print(names)
```

**Output:**

```
['Asha', 'Ravi', 'Meera']
```

Python shows strings inside a list with quotation marks. This tells you the elements are strings.

## Lists with Mixed Data Types

The elements of a list do not have to be the same type. Integers, floating-point numbers, strings and Booleans can appear together in the same list.

**Example**

```python
student = ["Asha", 87, 92.5, True]
print(student)
```

**Output:**

```
['Asha', 87, 92.5, True]
```

Each element keeps its own type. In practice a list usually holds values of one type, such as a list of marks or a list of names.

## The list Data Type

The elements have their own types, and the list itself also has a type: `list`. It is a built-in data type in Python, alongside `int`, `float`, `str` and `bool`.

**Example**

```python
marks = [87, 72, 95]
print(type(marks))
```

**Output:**

```
<class 'list'>
```

## Order of Elements

A list is ordered, which means its elements stay in the order you put them in unless the program changes that order.

**Example**

```python
marks = [95, 72, 87]
print(marks)
```

**Output:**

```
[95, 72, 87]
```

The highest mark was written first, so it stays first. The order is yours to decide.

## Duplicate Elements

The same value may appear in a list more than once.

**Example**

```python
marks = [87, 72, 87]
print(marks)
print(len(marks))
```

**Output:**

```
[87, 72, 87]
3
```

The value `87` appears twice, and the length is `3`. Each occurrence is a separate element, so both are counted.

## Lists of Zero and One Element

A list can contain zero elements, one element, or many elements.

**Example**

```python
marks = []
print(marks)
print(len(marks))

marks = [87]
print(marks)
print(len(marks))
```

**Output:**

```
[]
0
[87]
1
```

An empty list is useful when the values will be added later. A list with one element is still a list because the value is inside square brackets. Without the brackets, `marks = 87` would create an integer.

## Adding an Element to a List

The `append()` method adds one element to the end of a list.

**Syntax**

```
list_name.append(element)
```

**Example**

```python
marks = [87, 72]
marks.append(95)
print(marks)
```

**Output:**

```
[87, 72, 95]
```

The list had two elements and now has three. `append()` changes the existing list rather than creating a new one.

> **A method is an operation provided by a value and called using a dot after that value.** `marks.append(95)` calls the `append()` method on the list stored in `marks`.

`len(marks)` calls a function and gives it the list as an argument. `marks.append(95)` calls a method that belongs to the list.

A list has several other methods for adding and removing elements. They are covered in detail in a later topic.
## Building a List in a Loop

An empty list and `append()` let a loop keep every value it reads. A loop processes one value at a time, and the loop variable holds only the current value. Appending each value to a list keeps them all.

**Example**

```python
marks = []
for student in range(3):
    mark = int(input("Marks: "))
    marks.append(mark)

print(marks)
print("Students recorded:", len(marks))
```

**Output:**

```
Marks: 87
Marks: 72
Marks: 95
[87, 72, 95]
Students recorded: 3
```

`mark` was replaced on every iteration. The list kept each value as it arrived, so all three are present when the loop ends.

## Summary

The key points about lists:

- A list is an ordered collection of values stored in a single variable.
- Each value in a list is called an element.
- A list is written with square brackets, and the elements are separated by commas.
- `list` is a built-in data type, alongside `int`, `float`, `str` and `bool`.
- The elements of a list may be of any data type, and a single list may mix types.
- The elements stay in the order they were written unless the program changes that order.
- The same value can appear more than once, and each occurrence counts as a separate element.
- A list may hold zero elements, one element, or any number.
- `len()` returns the number of elements in a list.
- `append()` adds one element to the end of a list and changes the existing list.

## Further Reading

- 📎 **Lists with worked examples** — https://www.programiz.com/python-programming/list
- 📎 **Official Python guide to lists** — https://docs.python.org/3/tutorial/datastructures.html

A list holds many values, but you know only how to store them so far. How do you get one particular value—the first, last, or one in the middle?

Next, you will learn how to access individual elements using their position.
