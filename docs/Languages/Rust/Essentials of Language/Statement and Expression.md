---
layout: default
title: Statement and Expression
parent: Essentials of Language
grand_parent: Rust
nav_order: 1
--- 
# Statement and Expression
In Rust, the syntax is divided into two major categories: **Statements** and **Expressions**.

**Statements** can be divided into two categories: **Declaration statements** and **Expression statements**.

**Expression statements** specifically refer to statements ending with a semicolon.

When the compiler analyzes code, it will continue executing the code if it encounters a semicolon. If it encounters a statement, it will execute the statement. If it encounters an expression, it will calculate the value of the expression. If there is nothing after a semicolon, it will add a **unit value()** of the **unit type**.

When the compiler encounters a function in Rust, it recognizes the curly braces ('{}') as a block expression. A block expression consists of a pair of braces and a series of expressions within the block.
The value of a block expression in Rust is determined by the value of the last expression within the block. 

From this perspective, Rust treats everything as expressions. Even statements can be seen as expressions that evaluate to a unit value when there is nothing after the semicolon.