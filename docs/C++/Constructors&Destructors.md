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

## Prevent exceptions from leaving desturctors

### Destructors should never emit exceptions.

If functions called in a
destructor may throw, the destructor should catch any exceptions,
then swallow them or terminate the program.

```cpp
class Widget {
public:
    ...
    ~Widget() { ... } // assume this might emit an exception
};

void doSomething()
{
    std::vector<Widget> v;
    ...
}
```

There might be two simultaneously active exceptions and cause undefined behavior.

If your destructor needs to perform an operation that may fail by throwing an exception, for example:

```cpp
class DBConn
{
public: // objects
    ...
    // class to manage DBConnection
    ~DBConn() // make sure database connections
    { // are always closed
        db.close();
    }
private:
    DBConnection db;
};
```

There are two primary ways to avoid the trouble. DBConn’s destructor
could:

- Terminate the program if close throws, typically by calling abort:

```cpp
DBConn::~DBConn()
{
    try {
        db.close();
    }
    catch (...) {
    //make log entry that the call to close failed;
        std::abort();
    }
}
```

- Swallow the exception arising from the call to close:

```cpp
DBConn::~DBConn()
{
try { db.close(); }
catch (...) {
make log entry that the call to close failed;
}
}
```

### Provide regular functions that may throw exceptions

- If class clients need to be able to react to exceptions thrown during
  an operation, the class should provide a regular (i.e., non-destruc-
  tor) function that performs the operation.

```cpp
A better strategy is to design DBConn’s interface so that its clients have an opportunity to react to problems that may arise. For example, DBConn could offer a close function itself, thus giving clients a chance
to handle exceptions arising from that operation.
class DBConn {
public:
...
void close() // new function for
{ // client use
    db.close();
    closed = true;
}
{
~DBConn()
if (!closed) {
    try {
        db.close(); // close the connection
        // if the client didn’t
    }
    catch (...) { // if closing fails,
        make log entry that call to close failed; // note that and
        ... // terminate or swallow
    }
}
}
private:
    DBConnection db;
    bool closed;
};
```

## Never call virtual functions during construction or destruction

In the constructor:

When the base class's constructor is running, the derived class portion has not yet been fully constructed. Therefore, the dynamic type of the object is the base class type, not the derived class type. If a virtual function is called at this point, the base class's version will be invoked, not the derived class's version.

In the destructor:

When the derived class's destructor is running, the derived class portion has already been destroyed. Therefore, the dynamic type of the object reverts to the base class type. If a virtual function is called at this point, the base class's version will be invoked, not the derived class's version.

```cpp
#include <iostream>

class Base {
public:
    Base() {
        std::cout << "Base constructor\n";
        callVirtual(); // 调用虚函数
    }

    virtual void callVirtual() {
        std::cout << "Base::callVirtual\n";
    }

    virtual ~Base() {
        std::cout << "Base destructor\n";
        callVirtual(); // 调用虚函数
    }
};

class Derived : public Base {
public:
    Derived() {
        std::cout << "Derived constructor\n";
    }

    void callVirtual() override {
        std::cout << "Derived::callVirtual\n";
    }

    ~Derived() {
        std::cout << "Derived destructor\n";
    }
};

int main() {
    Derived d;
    return 0;
}
```

# Have assignment operators return a reference to \*this

This is only a convention; code that doesn’t follow it will compile. How-
ever, the convention is followed by all the built-in types as well as by
all the types in the standard library (e.g., string, vector, complex, tr1::shared_ptr, etc.). Unless you have a good reason for doing things differently, don’t.
