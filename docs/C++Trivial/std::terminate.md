---
layout: default
title: std::terminate
parent: C++ Trivial
nav_order: 1
---

# std::terminate

## When std::terminate will be automatically invoked

C++ will automatically invoke `std::terminate()` in the following situations:

1. An uncaught exception.
2. Throwing an exception in a `noexcept` function.
3. Throwing a second exception during stack unwinding (double exception).
4. Calling `std::rethrow_exception` or `throw;` when there is no active exception.
5. An exception that is not caught at the boundary of a thread.
6. An exception propagating outside the `main` function.
7. Calling an undefined pure virtual function.
8. Explicitly invoking `std::terminate()` in a destructor or other code.

## What is `std::terminate()`?

`std::terminate()` is a function in the C++ Standard Library that is called when the program can no longer continue to run normally. It is used to immediately terminate program execution. It is typically triggered in the following scenarios:

1. An exception is thrown, but no matching `catch` block is found.
2. An exception is thrown from a function declared as `noexcept`.
3. An exception is thrown during stack unwinding, and another exception is thrown (double exception).
4. `std::terminate()` is explicitly called.

When `std::terminate()` is invoked, the program terminates without returning to the point of the call. It also does not perform any stack cleanup operations (e.g., calling destructors).

---

## Default Behavior of `std::terminate()`

By default, `std::terminate()` invokes the **terminate handler**, which is `std::abort()` by default.

- **`std::abort()`**: Immediately terminates program execution, usually generating a core dump file for debugging.
- Developers can change the default behavior by setting a custom terminate handler.

---

## Custom Terminate Handler

C++ provides interfaces that allow developers to replace the default terminate handler using the following functions:

- **`std::set_terminate()`**: Sets a custom terminate handler function.
- **`std::get_terminate()`**: Retrieves the current terminate handler.

### Example

```cpp
#include <iostream>
#include <exception>
#include <cstdlib>

void customTerminate() {
    std::cerr << "Custom terminate handler called. Exiting...\n";
    std::abort(); // Or perform other cleanup actions before exiting
}

int main() {
    std::set_terminate(customTerminate);

    try {
        throw 42; // No catch block to handle this exception
    } catch (...) {
        std::cerr << "This won't be executed.\n";
    }

    return 0;
}
```

**Output**:

```
Custom terminate handler called. Exiting...
```

In the example above, the custom terminate handler `customTerminate` is set as the current handler. When an uncaught exception is thrown, calling `std::terminate()` executes the custom handler.

---

## Specific Implementation of `std::terminate()`

The C++ Standard does not define the exact implementation of `std::terminate()`; it depends on the compiler and runtime library. Below is a high-level description of a typical implementation process:

1. **Check for a custom terminate handler**:

   - If a custom terminate handler is set, call that handler.
   - If no custom handler is set, call the default terminate handler, `std::abort()`.

2. **Execute termination logic**:
   - `std::abort()` immediately terminates the program and may generate a core dump file.
   - Stack cleanup operations (e.g., destructors) are skipped.

### Example Pseudocode

```cpp
void std::terminate() {
    if (current_terminate_handler) {
        current_terminate_handler(); // Call the custom handler
    } else {
        std::abort(); // Call the default handler
    }
}
```

### Example Implementation in GCC

In GCC's `libstdc++`, `std::terminate()` is typically implemented in `terminate.cc` with logic similar to the following:

```cpp
namespace std {
    static terminate_handler __terminate_handler = nullptr;

    void terminate() noexcept {
        if (__terminate_handler) {
            __terminate_handler(); // Call the custom terminate handler
        } else {
            std::abort(); // Call the default terminate handler
        }
    }

    terminate_handler set_terminate(terminate_handler new_handler) noexcept {
        auto old_handler = __terminate_handler;
        __terminate_handler = new_handler;
        return old_handler;
    }
}
```

---

## Summary

- `std::terminate()` is the last resort in the exception handling mechanism, invoked when the program cannot continue execution safely.
- By default, it calls `std::abort()`, but you can override this behavior using `std::set_terminate()`.
- Its specific implementation varies by compiler and runtime library but generally follows the logic of invoking the terminate handler or `std::abort()`.
