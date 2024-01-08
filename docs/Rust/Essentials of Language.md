---
layout: default
title: Essentials of Languages
parent: Rust
nav_order: 1
---

Hi, here is Essentials of Language!

# Function and Closure

## Definition of Functions

In Rust, 'fn' is used to define a function.

```rust
pub fn add(a: i32, b:i32)->i32{
    a+b
}
pub fn fizz_buzz(num :i32)->String{
    if num % 15 == 0 {
        return "fizz_buzz".to_string(); // Explicit return statement
    }else{
        return "".to_string();
    }
}
```

As mentioned earlier, the body of a function in Rust is typically a block expression. A block expression is a series of expressions enclosed within curly braces ('{}'). The value of the block expression is determined by the value of the last expression within the block.

In Rust, the 'return' keyword is used to explicitly return a value from a function before reaching the end of the block. When 'return' is used, it immediately terminates the execution of the function and returns the specified value.

## Scope and Lifetime

Rust follows a static or lexical scoping rule, also known as **lexical scope**. The scope of a variable in Rust is defined by a pair of curly braces ('{}'). Variables declared within a scope are accessible only within that scope and any nested inner scopes. Once the control flow exits the scope, the variables are no longer accessible.

```rust
fn main(){
    let v = 1;
    assert_eq!(v, 1);
    {
        let v = 1;
        assert_eq!(v, 2);
    }
    assert_eq!(v, 1);
}
```

The lifetime of a binding variable begins at the point of binding and ends at the end of its lexical scope.

## Function Pointer

```rust
pub fn math(op: fn(i32, i32)-> i32, a : i32, b:i32)-> i32{
    op(a, b)
}

fn add(a : i32, b: i32)->i32{
    a+b
}

fn product(a : i32, b: i32)->i32{
    a*b
}

fn main(){
    let a = 2;
    let b = 3;
    assert_eq!(math(add,a,b),5);
    assert_eq!(math(product, a, b), 6);
}
```

Use the name of a function directly as its function pointer. In the example above, the type of the function pointers of function 'add' and 'product' is _fn(i32, i32)->i32_.

```rust
fn is_true()->bool{true}
fn true_maker()->fn()->bool{is_true}
fn main(){
    assert_eq!(true_maker()(), true);
}
```

## CTFE mechanism

Rust has an incomplete CTFE(Compile Time Function Execution) ability.

```rust
const fn init_len()->usize{
    return 5;
}

fn main(){
    let arr = [0; init_len()];
}
```

In the example above, the function 'init_len' has to be executed in the compile time.

## Closure

```rust
fn main(){
    let out = 42;
    fn add(i :i32, j : i32)->i32{
        i + j + out
    }
    let closure_annotated = |i : i32, j:i32|->i32{
        i+j+out
    }
    let closure_inferred = |i,j|i+j+out;
    let i = 1;
    let j = 2;
    assert_eq!(3, add(i,j));
    assert_eq!(45, closure_annotated(i,j));
    assert_eq!(45, closure_inferred(i,j));
}
```

There is huge difference between closures and functions. That is : closures can capture and access variables from their surrounding environment, which functions can't.

Closures can be parameters:

```rust
fn math<F : Fn() ->i32>(op: F)->i32{
    op()
}
fn main(){
    let a = 2;
    let b = 3;
    assert_eq!(math(|| a+b), 5);
    assert_eq!(math(|| a*b), 6);
}
```

Closure can also be returned values:

```rust
fn two_time_impl()-> impl Fn(i32)->i32{
    let i = 2;
    move |j| j*i
}
```

Fn is a trait. 'move' keyword transfers the ownership of the closure to prevent the reference of it from becoming dangling pointer.

# Statement and Expression

In Rust, the syntax is divided into two major categories: **Statements** and **Expressions**.

**Statements** can be divided into two categories: **Declaration statements** and **Expression statements**.

**Expression statements** specifically refer to statements ending with a semicolon.

When the compiler analyzes code, it will continue executing the code if it encounters a semicolon. If it encounters a statement, it will execute the statement. If it encounters an expression, it will calculate the value of the expression. If there is nothing after a semicolon, it will add a **unit value()** of the **unit type**.

When the compiler encounters a function in Rust, it recognizes the curly braces ('{}') as a block expression. A block expression consists of a pair of braces and a series of expressions within the block.
The value of a block expression in Rust is determined by the value of the last expression within the block.

From this perspective, Rust treats everything as expressions. Even statements can be seen as expressions that evaluate to a unit value when there is nothing after the semicolon.

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

It's important to note that the borrow operator '&' and the ability to create references are fundamental concepts in Rust's ownership and borrowing system. They enable safe and efficient memory management while providing a flexible mechanism for working with values.
