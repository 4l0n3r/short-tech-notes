cargo new project_name
cargo build  -> target/debug
cargo build --release  -> target/release
cargo run
cargo check
cargo update

:: -> associated function is a function that’s implemented on a type

crate is a collection of Rust source code files

match comparision
    arm-1 -> expression
    arm-2 -> expression

Shadowing

statement -> won't return anything  ( x = y = 6 will work in other lang, but not in rust since y = 6 is a statement and don't return anything )
expression -> return a value ( without semicolon )

Rust will not automatically try to convert non-Boolean types to a Boolean

shallow copy -> reference
hard copy -> data copy
move -> reference + delete only one

Copy trait that we can place on types that are stored on the stack
Rust won’t let us annotate a type with Copy if the type, or any of its parts, has implemented the Drop trait

At any given time, you can have either one mutable reference or any number of immutable references.
reference scope dhani last usage varakuu matrame untadhi

tring literals are string slices

struct
    Note that the entire instance must be mutable.Rust doesn’t allow us to mark only certain fields as mutable
    Field Init Shorthand
    ..user1 ( move )
    tuple structs ( Color, Point )
    Unit like structs
    {rect1:?} , {rect1:#?}

for (index,value) in nums.iter().enumerate()
    *value
