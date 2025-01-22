---
layout: default
title: Undefined Behavior
parent: C++ Trivial
nav_order: 2
---

# Categories of Undefined Behavior in C++

Undefined behavior (UB) in C++ occurs when the program violates the rules of the C++ Standard in ways that the Standard does not define. There are several common categories of undefined behavior, including:

---

## **1. Memory Violations**

- **Accessing invalid memory locations**:
  - Dereferencing a `nullptr`.
  - Accessing memory outside the bounds of an array or object.
  - Using a dangling pointer or reference (e.g., after the object has been deleted).
- **Double `delete` or deleting an invalid pointer**:
  ```cpp
  int* p = new int;
  delete p;
  delete p; // UB: double delete
  ```

---

## **2. Violations of Object Lifetime**

- **Accessing an object outside its lifetime**:
  - Using an object after it has been destroyed or before it has been constructed.
  - Calling a member function on a moved-from or destroyed object.

---

## **3. Violations of Type Safety**

- **Invalid type conversions**:
  - Casting an object to an unrelated type.
  - Reinterpreting memory with an incompatible type.
  - Using a pointer to an object as a pointer to an incorrect type.
  ```cpp
  int i = 42;
  char* p = reinterpret_cast<char*>(&i); // May cause UB
  ```

---

## **4. Data Races in Multithreading**

- **Concurrent modification and access**:
  - Two or more threads access the same memory location, and at least one is a write, without proper synchronization.
  ```cpp
  int shared_var = 0;
  std::thread t1([&] { shared_var++; });
  std::thread t2([&] { shared_var--; }); // Race condition: UB
  ```

---

## **5. Improper Use of Functions**

- **Violating function contracts**:
  - Calling functions with invalid arguments (e.g., passing a `nullptr` to a function that does not allow it).
  - Violating `noexcept` guarantees (leads to `std::terminate()` but not UB itself).
- **Calling a deleted or non-existent function**:
  - Calling a pure virtual function directly via a base class pointer.
  ```cpp
  struct Base {
      virtual void func() = 0; // Pure virtual function
  };
  Base* b = nullptr;
  b->func(); // UB: calling a pure virtual function
  ```

---

## **6. Unsequenced Modifications**

- **Modifying an object multiple times without a sequence point**:
  - Undefined behavior occurs when the order of modifications is not specified.
  ```cpp
  int x = 0;
  x = x++ + ++x; // UB: unsequenced modifications of x
  ```

---

## **7. Violations of Compiler Optimizations**

- **Assumptions made by the compiler**:
  - Modifying a `const` object.
  - Violating `strict aliasing rules` by accessing the same memory location using pointers of incompatible types.
  ```cpp
  int x = 42;
  float* p = (float*)&x; // UB: violates strict aliasing
  ```

---

## **8. Standard Library Misuse**

- **Improper use of library functions**:
  - Providing invalid iterators to standard algorithms.
  - Modifying a container while iterating over it.
  ```cpp
  std::vector<int> vec = {1, 2, 3};
  for (auto it = vec.begin(); it != vec.end(); ++it) {
      vec.push_back(4); // UB: modifying container during iteration
  }
  ```

---

## **9. Violations in Low-Level Operations**

- **Using uninitialized variables**:
  - Reading the value of an uninitialized variable.
  ```cpp
  int x;
  int y = x + 1; // UB: x is uninitialized
  ```
- **Shift operations with invalid ranges**:
  - Shifting a value by more bits than its width.
  ```cpp
  int x = 1 << 32; // UB: shift exceeds width of int
  ```

---

## **10. UB in Exception Handling**

- **Throwing exceptions in `noexcept` functions**.
- **Uncaught exceptions during stack unwinding**:
  - Throwing another exception while an exception is being propagated.

---

## **11. Use of UB-Prone Features**

- **Inline assembly with no guarantees**:
  - Behavior depends on compiler and architecture.
- **Breaking `volatile` or `atomic` rules**:
  - Misuse of `volatile` or incorrect atomic operations may lead to UB.

---

## Summary

Undefined behavior in C++ can occur in many ways, often related to:

1. Memory issues.
2. Object lifetime violations.
3. Type safety violations.
4. Unsequenced or unsynchronized operations.
5. Improper use of functions, standard library, or low-level operations.

Understanding these categories helps write safer, more predictable C++ code. Tools like sanitizers (e.g., AddressSanitizer, ThreadSanitizer) and static analyzers can detect potential UB in your code.
