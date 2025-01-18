---
layout: default
title: The Semantics of Inheritance
parent: C++
nav_order: 4
---

# Public Inheritance (is-a)

- Meaning: The derived class is a specialized version of the base class.
- Use case: When the derived class should expose the public interface of the base class to external users, maintaining an “is-a” relationship.
- Example: A Dog class inheriting from an Animal class.

# Protected Inheritance (is-implemented-in-terms-of for subclasses)

- Meaning: The derived class uses the base class for its implementation, and this relationship is visible to its subclasses but not to external users.
- Use case: When the base class implementation should be shared among the derived class and its further subclasses, but not exposed publicly.
- Example: A Car class inheriting from an Engine class with a design intent that further car types (e.g., SportsCar) can access the engine's functionality.

# Private Inheritance (is-implemented-in-terms-of)

- Meaning: The derived class uses the base class for its implementation, but this relationship is entirely hidden from external users and subclasses.
- Use case: When the base class functionality is purely an implementation detail for the derived class and should not affect the derived class’s public interface or inheritance hierarchy.
- Example: A HomeForSale class inheriting from Uncopyable to prevent copying while hiding the fact that it uses Uncopyable.
