cargo new project_name
cargo build  -> target/debug
cargo build --release  -> target/release
cargo run
cargo check
cargo update

Variables & Mutability
    let x = 5;        // Immutable
    let mut y = 10;    // Mutable
    y += 1;            // Allowed
    // x += 1;         // Error: x is immutable

Shadowing
    let x = 5;
    let x = x + 1; // New variable with same name

match
    let coin = "Penny";
    match coin {
        "Penny" => println!("1 cent"),
        "Nickel" => println!("5 cents"),
        _ => println!("Unknown coin"),
    }

loops
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

Arrays
    let arr = [1, 2, 3, 4, 5];
    let first = arr[0]; // Accessing elements
    let second = arr[1];
    
    for element in a {
        println!("Value: {element}");
    }

vectors
    let mut v: Vec<i32> = Vec::new();  // Empty vector
    v.push(5);
    v.push(6);

    let v2 = vec![1, 2, 3];           // Macro initialization
    
    let third = &v2[2];      // Panics if out of bounds
    let third = v2.get(2);   // Returns Option<&i32>

    for i in &v2 {
        println!("{i}");
    }

strings
    let s1 = "Hello";             // String slice (&str)
    let s2 = String::from("Hello"); // Growable String
    let s3 = s2 + " World!";      // Concatenation
    let sub = &s1[0..2];          // Slicing ("He")

struct
    Note that the entire instance must be mutable.Rust doesn’t allow us to mark only certain fields as mutable
    Field Init Shorthand
    ..user1 ( move )
    tuple structs ( Color, Point )
    Unit like structs
    {rect1:?} , {rect1:#?}

traits
    #[derive(Debug, Clone, Copy)]
    struct Point {
    x: i32,
    y: i32,
    }
    
    let p1 = Point { x: 0, y: 0 };
    let p2 = p1;  // OK because of Copy trait
    println!("{:?}", p1);  // Debug print    

Ownership basics
    let s1 = String::from("hello");
    let s2 = s1;                  // s1 is moved (invalidated)
    // println!("{}", s1);        // Error!
    
    let s3 = s2.clone();          // Deep copy
    let len = calculate_len(&s3); // Borrowing
    
    fn calculate_len(s: &String) -> usize {
        s.len()
    }

:: -> associated function is a function that’s implemented on a type

crate is a collection of Rust source code files

statement -> won't return anything  ( x = y = 6 will work in other lang, but not in rust since y = 6 is a statement and don't return anything )
expression -> return a value ( without semicolon )

Rust will not automatically try to convert non-Boolean types to a Boolean

shallow copy -> reference
hard copy -> data copy
move -> reference + delete the old one

Copy trait that we can place on types that are stored on the stack
Rust won’t let us annotate a type with Copy if the type, or any of its parts, has implemented the Drop trait

At any given time, you can have either one mutable reference or any number of immutable references.
reference scope dhani last usage varakuu matrame untadhi

string literals are string slices

for (index,value) in nums.iter().enumerate()
    *value





