# CSP Solver

[![Crates.io](https://img.shields.io/crates/v/cspsolver.svg?color=blue)](https://crates.io/crates/cspsolver)
[![Documentation](https://docs.rs/cspsolver/badge.svg)](https://docs.rs/cspsolver)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A Constraint Satisfaction Problem (CSP) solver library written in Rust.

## Overview

This library provides efficient algorithms and data structures for solving constraint satisfaction problems. CSPs are mathematical problems defined as a set of objects whose state must satisfy a number of constraints or limitations.

**Variable Types**: `int`, `float`, mixed constraints

**Constraint Categories**:
- **Mathematical**: `+`, `-`, `*`, `/`, `%`, `abs()`, `min()`, `max()`, `sum()`
- **Comparison**: `==`, `!=`, `<`, `<=`, `>`, `>=` (natural syntax)
- **Boolean Logic**: `and()`, `or()`, `not()` with clean function syntax
- **Global**: `alldiff()`

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
    let z = model.float(0.0, 20.0); // Float variable

    // Mathematical constraints using post! macro
    post!(model, x < y);            // Comparison
    post!(model, x + y >= int(10)); // Arithmetic
    post!(model, abs(z) <= float(15.5)); // Math functions
    
    // Enhanced constraint features
    post!(model, sum([x, y]) == int(12));     // Sum function
    post!(model, and(x > int(3), y < int(12))); // Boolean logic
    post!(model, x % int(3) != int(0));       // Modulo operations
    
    // Global constraints
    post!(model, alldiff([x, y]));  // All different

    if let Some(solution) = model.solve() {
        println!("x = {:?}", solution[x]);
        println!("y = {:?}", solution[y]);
        println!("z = {:?}", solution[z]);
    }
}
```

### Advanced Constraint Examples

```rust
use cspsolver::prelude::*;

fn main() {
    let mut model = Model::default();
    let vars = vec![model.int(1, 5), model.int(1, 5), model.int(1, 5)];
    
    // Complex mathematical expressions
    post!(model, sum(vars.clone()) <= int(12));
    post!(model, max([vars[0]]) >= min([vars[1]]));
    
    // Boolean logic with traditional syntax  
    let a = model.int(0, 1);
    let b = model.int(0, 1);
    post!(model, and(a, b));        // Boolean AND
    post!(model, or(a, not(b)));    // Boolean OR with NOT
    
    // Mixed type constraints
    let float_var = model.float(1.0, 10.0);
    post!(model, abs(float_var) + vars[0] <= float(15.0));
    
    if let Some(solution) = model.solve() {
        println!("Solution found!");
    }
}
```

## Status

The library is currently in active development. Features and APIs may change as we refine the implementation and add new functionality.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

