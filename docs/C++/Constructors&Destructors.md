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
