---
layout: default
title: Replace Define
parent: C++
nav_order: 2
---

#### _January 1st, 2023_

# For simple constants, prefer const objects or enums to #defines

## const

For example

```cpp
#define ASPECT_RATIO 1.653
```

The symbolic name ASPECT_RATIO may never be seen by compilers; it may be removed by the preprocessor before the source code ever gets to a compiler. As a result, the name ASPECT_RATIO may not get entered into the **symbol table**. This can be confusing if you get an error during compilation involving the use of the constant, because the error mes- sage may refer to 1.653, not ASPECT_RATIO. This is the first reason why we prefer const objects or enums to #defines.

The solution is:

```cpp
const double AspectRatio = 1.653;
```

The another reason why we prefer const objects than macros, is the use of #define may yield larger code. That’s because the preprocessor’s blind substitution of the macro name ASPECT_RATIO with 1.653 could result in multiple copies of 1.653 in your object code, while the use of the constant AspectRatio should never result in more than one copy.

The third reason it that there’s no way to create a class-specific con- stant using a #define, because #defines don’t respect scope. To limit the scope of a constant to a class, you could make it a member, and to ensure there’s at most one copy of the constant, you have to make it a static member:

```cpp
class GamePlayer {
private:
    static const int NumTurns = 5;
    int scores[NumTurns];
    ...
};
```

## enums

```cpp
class GamePlayer {
private:
    enum { NumTurns = 5 };
    int scores[NumTurns];
    ...
};
```

The enum hack is worth knowing about for several reasons. First, the enum hack behaves in some ways more like a #define than a const does, and sometimes that’s what you want. For example, it’s legal to take the address of a const, but it’s not legal to take the address of an enum, and it’s typically not legal to take the address of a #define, either.Also, though good compilers won’t set aside storage for const objects of integral types (unless you create a pointer or reference to the object), sloppy compilers may, and you may not be willing to set aside memory for such objects. Like #defines, enums never result in that kind of unnecessary memory allocation.

A second reason to know about the enum hack is purely pragmatic.

# For function-like macros, prefer inline functions to #defines

Use #define to implement functions leads to problems, such as:

```cpp
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))

inta=5,b=0;
CALL_WITH_MAX(++a, b); // a is incremented twice
CALL_WITH_MAX(++a, b+10); //a is incremented once
```

You can get all the efficiency of a macro plus all the predictable behavior and type safety of a regular function by using a template for an inline function

```cpp
template<typename T>
inline void callWithMax(const T& a, const T& b) {
    f(a>b?a:b);
}
```
