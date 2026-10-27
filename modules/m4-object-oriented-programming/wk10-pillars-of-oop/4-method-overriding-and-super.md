# Method Overriding and `super()`

A child class inherits methods from its parent class and can use them exactly as they are. Sometimes, however, an inherited method does not suit the child class, and the child needs it to work differently. Changing the method inside the parent class would also affect the other classes that inherit and use it. Creating a separate method with a different name would require callers to use a different method name for the same general action.

Object-oriented programming solves this through **method overriding**. A child class can write its own version of an inherited method, while the parent class stays unchanged.

> **Method overriding is a feature in which a child class provides its own implementation of a method that is already defined in its parent class, using the same method name. It allows the child class to change inherited behaviour without modifying the parent class.**

The parent's version is called the **overridden method**, and the child's version is called the **overriding method**.

## Overriding a Method

To override a method, define a method with the **same name** in the child class.

```python
class Person:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        print("My name is", self.name)

class Student(Person):
    def introduce(self):
        print("My name is", self.name, "and I am a student")

person1 = Person("Arun")
student1 = Student("Ravi")

person1.introduce()
student1.introduce()
```

**Output:**

```
My name is Arun
My name is Ravi and I am a student
```

- `student1.introduce()` runs the version in `Student`, because Python searches the object's own class first and finds `introduce()` there.
- `person1.introduce()` runs the version in `Person`. **Overriding does not change the parent class.**

The method name must match exactly, and the child method should accept the arguments that code calling it is expected to pass.

The same method call can produce different behaviour depending on the object. This is called **runtime polymorphism**, which you'll explore in the next topic, **Polymorphism**.

## Overriding the `__init__()` Method

`__init__()` can be overridden like any other method. A child class does this when its objects need extra data, such as a roll number for each student.

```python
class Person:
    def __init__(self, name):
        self.name = name

class Student(Person):
    def __init__(self, name, roll_number):
        self.name = name
        self.roll_number = roll_number

student1 = Student("Ravi", 101)
print(student1.name, student1.roll_number)
```

**Output:**

```
Ravi 101
```

Because `Student` defines its own `__init__()`, Python does not automatically call `Person.__init__()`. The student class must initialize `name` itself, **repeating code already present in the parent**. How can we reuse that code instead? Python provides `super()` for this purpose.

## The `super()` Function

When a child class overrides a method, it replaces the inherited behaviour for that method call. But sometimes, the child needs to reuse the parent's behaviour and add something extra.

Python provides the built-in **`super()`** function for this purpose. **It allows a child class to call the next implementation of a method in the inheritance order.**

**Example:**

```python
class Person:
    def introduce(self):
        print("I am a person")

class Student(Person):
    def introduce(self):
        super().introduce()
        print("I am a student")

student1 = Student()
student1.introduce()
```

**Output:**

```
I am a person
I am a student
```

**How it works:**

- `super().introduce()` calls the next implementation of `introduce()` in the inheritance order. In this example, that implementation is in `Person`.
- The next line adds the student's own behaviour.

Without `super().introduce()`, only the code inside `Student.introduce()` would run.

### Using `super()` with `__init__()`

A child class can also use `super()` to reuse the parent's initialization code instead of repeating it.

```python
class Person:
    def __init__(self, name):
        self.name = name

class Student(Person):
    def __init__(self, name, roll_number):
        super().__init__(name)
        self.roll_number = roll_number

student1 = Student("Ravi", 101)
print(student1.name, student1.roll_number)
```

**Output:**

```
Ravi 101
```

Here, `super().__init__(name)` calls the next implementation of `__init__()` in the inheritance order, which is `Person.__init__()` in this example. The child then stores its additional attribute, `roll_number`.

> **Remember:** Use `super()` when a child class needs to reuse an inherited method rather than repeat its code.

## Points to Remember

- **Method overriding** lets a child class provide its own version of an inherited method, without changing the parent class.
- **`super()`** calls the next implementation of a method in the inheritance order, so the child can reuse it instead of repeating its code.
- **`super()` follows the MRO** to determine the next implementation to call, including in multiple inheritance.

## Further Reading

- 📎 **Method overriding in Python** — https://www.tutorialspoint.com/python/python_method_overriding.htm
- 📎 **Official Python reference for `super()`** — https://docs.python.org/3/library/functions.html#super

In the next topic, **Polymorphism**, you'll explore how the same method call can produce different behaviour for different types of objects.
