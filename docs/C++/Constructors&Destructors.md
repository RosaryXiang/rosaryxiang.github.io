---
layout: default
title: Constructors and Destructors
parent: C++
nav_order: 5
---

# Constructors and Destructors

## Six Special Member Functions

- Constructor
- Destructor
- Copy Constructor
- Copy Assignment Operator
- Move Constructor
- Move Assignment Operator

## Functions Silently Writes and Calls

Compilers may implicitly generate a class’s default constructor, copy constructor, copy assignment operator, and destructor.

If you declare no constructors at all, compilers will also declare a default constructor for you. All these functions will be both public and inline.

As for the copy constructor and the copy assignment operator, the compiler-generated versions simply copy each non-static data member of the source object over to the target object.

If you want to support copy assignment in a class containing a **reference** member, you must define the copy assignment operator yourself. Compilers behave similarly for classes containing **const** members.

Compilers reject implicit copy assignment operators in derived classes that inherit from base classes declaring the copy assignment operator private. After all, compiler-generated copy assignment operators for derived
classes are supposed to handle base class parts, too.

## Declare destructors vitual in polymorphic base classes

Polymorphic base classes should declare virtual destructors. If a class has any virtual functions, it should have a virtual destructor.

Classes not designed to be base classes or not designed to be used polymorphically should not declare virtual destructors.

You must provide a definition for the pure virtual destructor.
