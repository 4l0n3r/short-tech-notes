# Rust Quick Reference

## Cargo Commands
```
cargo new project_name
cargo build           # Builds debug version (target/debug)
cargo build --release # Builds release version (target/release)
cargo run
cargo check
cargo update
```

---

## Variables & Mutability
```rust
let x = 5;        // Immutable
let mut y = 10;   // Mutable
y += 1;           // Allowed
// x += 1;        // Error: x is immutable
```

---

## Shadowing
```rust
let x = 5;
let x = x + 1; // New variable with same name
```

---

## Match Statement
```rust
let coin = "Penny";
match coin {
    "Penny" => println!("1 cent"),
    "Nickel" => println!("5 cents"),
    _ => println!("Unknown coin"),
}
```

---

## Loops
```rust
// Infinite loop
loop {
    break;
}

// While loop
while x < 5 {
    x += 1;
}

// For loop
for i in 1..5 {
    println!("{}", i);
}

// For loop with enumerate
for (index, value) in nums.iter().enumerate() {
    println!("Index: {}, Value: {}", index, value);
}
```

---

## Arrays
```rust
let arr = [1, 2, 3, 4, 5];
let first = arr[0]; // Accessing elements
let second = arr[1];

for element in arr {
    println!("Value: {element}");
}
```

---

## Vectors
```rust
let mut v: Vec<i32> = Vec::new();  // Empty vector
v.push(5);
v.push(6);

let v2 = vec![1, 2, 3];           // Macro initialization

let third = &v2[2];      // Panics if out of bounds
let third = v2.get(2);   // Returns Option<&i32>

for i in &v2 {
    println!("{i}");
}
```

---

## Strings
```rust
let s1 = "Hello";                 // String slice (&str)
let s2 = String::from("Hello");   // Growable String
let s3 = s2 + " World!";          // Concatenation
let sub = &s1[0..2];              // Slicing ("He")
```

---

## Structs
- The entire instance must be mutable. Rust doesn’t allow marking only certain fields as mutable.
- Field Init Shorthand: `..user1` (move)
- Tuple structs: `Color`, `Point`
- Unit-like structs
- Debug print: `{rect1:?}`, `{rect1:#?}`

---

## Traits
```rust
#[derive(Debug, Clone, Copy)]
struct Point {
    x: i32,
    y: i32,
}

let p1 = Point { x: 0, y: 0 };
let p2 = p1;  // OK because of Copy trait
println!("{:?}", p1);  // Debug print
```

---

## Ownership Basics
```rust
let s1 = String::from("hello");
let s2 = s1;                  // s1 is moved (invalidated)
// println!("{}", s1);        // Error!

let s3 = s2.clone();          // Deep copy
let len = calculate_len(&s3); // Borrowing

fn calculate_len(s: &String) -> usize {
    s.len()
}
```

---

## Associated Functions
`::` -> Associated function is a function that’s implemented on a type

---

## Crate
A crate is a collection of Rust source code files.
- **Binary crate**: Can be many inside the `bin` folder.
- **Library crate**: Single

---

## Package
A package is one or more crates.

---

## Modules
### Basic
```rust
// In src/lib.rs or any module file
mod network {
    fn connect() {
        // function implementation
    }
}
```

### File-based
```rust
// src/network.rs
pub fn connect() {
    // public function
}

// src/lib.rs
mod network; // Load contents from network.rs
```

### Re-exporting
```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use front_of_house::hosting; // Re-export
```

### Visibility
```rust
mod outer {
    pub mod inner {
        pub(self) fn private_fn() {} // Only in inner
        pub(super) fn parent_fn() {} // Available to outer
        pub(crate) fn crate_fn() {} // Available in crate
    }

    pub fn example() {
        inner::parent_fn(); // Allowed
        // inner::private_fn(); // Error!
    }
}
```

---

## Statements vs Expressions
- **Statement**: Won't return anything. (`x = y = 6` will work in other languages, but not in Rust since `y = 6` is a statement and doesn't return anything)
- **Expression**: Returns a value (without semicolon)

---

## Boolean Conversion
Rust will not automatically try to convert non-Boolean types to a Boolean.

---

## Copy, Move, and Clone
- **Shallow copy**: Reference
- **Hard copy**: Data copy
- **Move**: Reference + delete the old one
- **Copy trait**: Can be placed on types stored on the stack. Rust won’t let us annotate a type with Copy if the type, or any of its parts, has implemented the Drop trait.

---

## References & Borrowing
- At any given time, you can have either one mutable reference or any number of immutable references.
- Reference scope lasts until the last usage.

---

