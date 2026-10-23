# Introduction to the Four Pillars of OOP

Classes and objects let a program group data and behaviour together. As programs grow, they also need to protect their data, reuse code, handle different objects in the same way, and hide unnecessary details. Object-oriented programming meets these needs through four main principles, called the **four pillars of OOP**.

> **The four pillars of OOP are encapsulation, inheritance, polymorphism and abstraction. They are the main principles used to design programs with classes and objects.**

## The Four Pillars

| Pillar | Meaning | Main Benefit |
| --- | --- | --- |
| **Encapsulation** | Keeping an object's data and methods together, and controlling how the data is accessed | Protects data from invalid changes |
| **Inheritance** | Creating a new class that reuses the attributes and methods of an existing class | Avoids repeating code |
| **Polymorphism** | Using the same method call or operation with different kinds of objects, each behaving in its own way | Lets one piece of code work with many types of objects |
| **Abstraction** | Showing only the essential features of an object and hiding how they work | Keeps code simple to use |

## The Pillars Through a Smartphone

A smartphone shows all four ideas:

- **Encapsulation:** The phone's internal parts are sealed inside its case. You change settings only through the screen, which stops you from damaging the parts directly.
- **Inheritance:** A new phone model keeps the features of the previous model and adds new ones, instead of being designed from nothing.
- **Polymorphism:** The same action, pressing the share button, works differently depending on what you share: a photo, a link or a document.
- **Abstraction:** You tap an icon to make a call. You do not need to know how the phone sends the signal.

## How the Pillars Work Together

```mermaid
flowchart LR
    A["Encapsulation"] --> B["Inheritance"]
    B --> C["Polymorphism"]
    C --> D["Abstraction"]
```

The pillars build on each other:

1. **Encapsulation** keeps each class's data and methods together and protected.
2. **Inheritance** lets new classes reuse an existing class.
3. **Polymorphism** uses inheritance to let child classes respond to the same method in their own way.
4. **Abstraction** defines what related classes must do, while each class decides how to do it.

This week covers each pillar in this order, followed by **Dunder Methods** and **Decorators**, which add more control over how classes behave.

## Further Reading

- 📎 **Official Python tutorial on classes** — https://docs.python.org/3/tutorial/classes.html

The first pillar protects an object's data from invalid changes. In the next topic, **Encapsulation**, you'll learn how a class controls access to its own data.
