---
layout: default
title: Initialization [Coding Convention]
parent: C++
nav_order: 3
---

#### _January 15th, 2025_

# Initialization

## Preview

**Make sure that objects are initialized before they're used.**

✦ **Manually initialize objects of built-in type**, because C++ only sometimes initializes them itself.
✦ In a constructor, **prefer use of the member initialization list** to assignment inside the body of the constructor. List data members in the initialization list in the same order they’re declared in the class.
✦ Avoid initialization order problems across translation units by **replacing non-local static objects with local static objects**.

## Details

#### Item 1： Initialization for built-in type

An array (from the C part of C++) isn’t necessarily guaranteed to have its contents initialized, but a vector (from the STL part of C++) is.

#### Item 2: Initialization for classes

Data members of an object are initialized before the body of a constructor is entered.

Members' default constructors are automatically called prior to entering the body of the class's constructor.

For most types, a single call to a copy constructor is more efficient — sometimes much more efficient — than a call to the default constructor followed by a call to the copy assignment operator.

Data members that are const or are references must be initialized; they can’t be assigned.

Base classes are initialized before derived classes, and within a class, data members are initialized in the order in which they are declared. This is true even if they are listed in a different order on the member initialization list.

#### Item 3: Initialization for static non-local objects

The order of initialization of non-local static objects defined in different translation units.

A **static object** is one that exists from the time it’s constructed until the end of the program.

Static objects inside functions are known as **local** static objects.

A **translation unit** is the source code giving rise to a single object file.

**_the relative order of initialization of non-local static objects defined in different translation units is undefined._**

Any kind of non-const static object — local or non-local — is trouble waiting to happen in the presence of multiple threads. One way to deal with such trouble is to manually **invoke all the reference-returning functions during the single-threaded startup portion** of the program. This eliminates initialization-related race conditions.
