---
layout: default
title: Variable and Binding
parent: Essentials of Language
grand_parent: Rust
nav_order: 2
--- 
# Variable and Binding
## Place and Value Expressions
Expressions in Rust can indeed be categorized into two categories: **Place expressions** and **Value expressions**, which correspond to the concepts of LValue and RValue in other languages.

**Place expressions** and **Value expressions** exist within their respective contexts: **Place context** and **Value context**.

By distinguishing between place context and value context, Rust ensures that the appropriate expressions are used in the corresponding contexts, enforcing ownership and borrowing rules to maintain memory safety and prevent data races.

## Binding
```rust
let a = 1;
// a = 2; immutable and error
let mut a = 1;
a = 2; // mutable
```
In Rust, the 'let' keyword is used to define a binding relationship between an identifier and a value. By default, when using 'let' to declare a binding, the place expression is immutable, meaning its value cannot be changed. However, by using 'let mut', the place expression can be declared as mutable, allowing its value to be modified.
```rust
let a= 1;
let a = 2; // variable shadowing
```
Repeatedly using the 'let' keyword to define a variable with the same name within the same scope is known as **variable shadowing**. The value of the variable is determined by the newest definition.

### Move
When a place expression appears on the right-hand side of an assignment operator ('='), it will transfer ownership of the place expression on the left-hand side, including the actual value and its memory address. This is known as the **Move** syntax. After the assignment, the place expression on the right-hand side cannot be accessed or visited anymore because its ownership has been transferred.

```rust
let a = 1;
let b = a; // Move
```

This move semantics in Rust helps enforce memory safety and prevents issues like use-after-free or double-free by ensuring that only one owner has access to a particular value at any given time.

### Copy
In Rust, the borrow operator '&' is used to create references. It allows you to borrow the value of an expression and obtain a reference to it. When borrowing a value using '&', you are creating a reference that points to the original value. This reference can be used in place of the original value and allows you to access or manipulate the value indirectly.

```rust
let a = 1;
let b = &a; // Copy
let mut c = 1;
let d = &mut c; // mutable copy
```

The borrow operator '&' is used to achieve the borrowing semantics, which is different from the **Copy** syntax. Borrowing allows you to pass a reference to a value instead of transferring ownership. It enables multiple references to access the same value concurrently, while still enforcing Rust's ownership and borrowing rules to prevent data races and ensure memory safety.

The '&mut' syntax is used to create mutable references. It is used in conjunction with the 'let mut' declaration to indicate that the reference obtained is mutable, allowing you to modify the value through that reference.

It's important to note that the borrow operator '&' and the ability to create references are fundamental concepts in Rust's ownership and borrowing system. They enable safe and efficient memory management while providing a flexible mechanism for working with values.s