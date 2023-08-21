---
layout: default
title: Function and Closure
parent: Essentials of Language
grand_parent: Rust
nav_order: 3
--- 
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
Use the name of a function directly as its function pointer. In the example above, the type of the function pointers of function 'add' and 'product' is *fn(i32, i32)->i32*.

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