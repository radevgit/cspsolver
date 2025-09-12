# CSP Solver

[![Crates.io](https://img.shields.io/crates/v/cspsolver.svg?color=blue)](https://crates.io/crates/cspsolver)
[![Documentation](https://docs.rs/cspsolver/badge.svg)](https://docs.rs/cspsolver)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A Constraint Satisfaction Problem (CSP) solver library written in Rust.

## Overview

This library provides efficient algorithms and data structures for solving constraint satisfaction problems. CSPs are mathematical problems defined as a set of objects whose state must satisfy a number of constraints or limitations.

Type of variables: `float`, `int`, `mixed` (int and float)

Constraints supported include:
- **Arithmetic**: `add`, `sub`, `mul`, `div`, `modulo`, `abs`, `sum`
- **Comparisons**: `lt`, `le`, `gt`, `ge`, `eq`, `ne` (short names for < ≤ > ≥ = ≠)
- **Boolean Logic**: Cean syntax with `&`, `|`, `!` operators (AND/OR/NOT operations)
- **Global**: `all_different`, `min`, `max` (vector-based)
- **Mixed Types**: Full support for integer and float variables in all constraints

## Installation

Add this to your `Cargo.toml`:

```toml
[dependencies]
cspsolver = "0.3.15"
```


## Examples

```bash
cargo run --example sudoku
cargo run --example pc_builder
cargo run --example resource_allocation
cargo run --example portfolio_optimization
```

```
🧩 Solving PLATINUM puzzle:
📊 Puzzle stats: 17 clues given, 64 empty cells

Puzzle:                                 Solution:
┌───────┬───────┬───────┐               ┌───────┬───────┬───────┐
│ · · · │ · · · │ · · · │               │ 9 8 7 │ 6 5 4 │ 3 2 1 │
│ · · · │ · · 3 │ · 8 5 │               │ 2 4 6 │ 1 7 3 │ 9 8 5 │
│ · · 1 │ · 2 · │ · · · │               │ 3 5 1 │ 9 2 8 │ 7 4 6 │
├───────┼───────┼───────┤               ├───────┼───────┼───────┤
│ · · · │ 5 · 7 │ · · · │               │ 1 2 8 │ 5 3 7 │ 6 9 4 │
│ · · 4 │ · · · │ 1 · · │               │ 6 3 4 │ 8 9 2 │ 1 5 7 │
│ · 9 · │ · · · │ · · · │               │ 7 9 5 │ 4 6 1 │ 8 3 2 │
├───────┼───────┼───────┤               ├───────┼───────┼───────┤
│ 5 · · │ · · · │ · 7 3 │               │ 5 1 9 │ 2 8 6 │ 4 7 3 │
│ · · 2 │ · 1 · │ · · · │               │ 4 7 2 │ 3 1 9 │ 5 6 8 │
│ · · · │ · 4 · │ · · 9 │               │ 8 6 3 │ 7 4 5 │ 2 1 9 │
└───────┴───────┴───────┘               └───────┴───────┴───────┘

✅ Solution found in 144330.511ms!
📊 Statistics: 638 propagations, 54 nodes explored
🔍 Efficiency: 11.8 propagations/node

```



### Basic Usage

```rust
use cspsolver::prelude::*;

fn main() {
    let mut model = Model::default();

    // Create variables with clean syntax
    let x = model.int(1, 10);       // Integer variable
    let y = model.int(5, 15);       // Integer variable  
    let flag = model.bool();        // Boolean variable (NEW!)

    // Arithmetic and comparison constraints
    model.post(x.lt(y));            // x < y
    model.post(x.ge(3));            // x >= 3
    
    // ✨ NEW: Ultra-clean boolean constraints
    model.post(flag & (x.gt(5)));   // Boolean AND
    model.post(flag | (y.lt(12)));  // Boolean OR
    model.post(!flag);              // Boolean NOT
    
    // Complex boolean expressions work seamlessly
    model.post((flag & (x.eq(7))) | (!flag & (y.eq(10))));
    
    // Batch operations for multiple constraints
    model.post_all(vec![
        x.ne(y),                    // x ≠ y
        flag & (x.lt(8)),          // Boolean constraint
        y.ge(6)                     // y ≥ 6
    ]);

    if let Some(solution) = model.solve() {
        println!("x = {:?}", solution[x]);
        println!("y = {:?}", solution[y]);
        println!("flag = {:?}", solution[flag]);
    }
}
```

### Ultra-Clean Boolean Logic

The new boolean syntax is incredibly clean and intuitive:

```rust
use cspsolver::prelude::*;

fn main() {
    let mut model = Model::default();
    let a = model.bool();           // Clean variable creation
    let b = model.bool();
    let c = model.bool();
    
    // Direct boolean operations - no verbose method calls!
    model.post(a & b);              // Boolean AND
    model.post(a | c);              // Boolean OR  
    model.post(!a);                 // Boolean NOT
    model.post((a & b) | (!c));     // Complex expressions
    
    // True batch operations
    model.post_all(vec![
        a & b,                      // Clean boolean expressions
        !c,                         // No .into() needed!
        a | (b & c)                 // Complex expressions work
    ]);
    
    // Before: model.bool_and(&[a, b])     ❌ Verbose
    // After:  model.post(a & b)           ✅ Clean!
}
```
    model.eq(y, int(8));   // instead of equals
    model.ne(x, y);        // instead of not_equals
    
    let solution = model.solve().unwrap();
    println!("x = {:?}, y = {:?}", solution[x], solution[y]);
}
```


## Status

The library is currently in active development. Features and APIs may change as we refine the implementation and add new functionality.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

