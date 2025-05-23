Intro:
    supports floating point numbers
    follows Interface Definition Language - ABI in solidity
    attributes ->
        #[program]
        #[error_code]
        #[derive(Accounts)]
        #[derive(Debug)] -> to print
    stops transactions:
        solidity -> triggers reverts
        solana -> returns an error
    mostly returns Result -> ( Ok / Error )
    does not have objects or classes
    by default it's upgradable -> program id won't get changed only code will be overwritten
    Solana does not have delegatecall
    no immutable variables ( sets on constructor )
    we can run tests without re-deploying the program -> program id + json file + provider

Basic Rust
    supports only fixed array, for dynamic we have vectors
    length -> usize
    Rust does not have try catch.
    
    copy types -> fixed size
    Ownership:
        many read references / one write reference. not both in same scope.
    Ownership is only an issue with non-copy types
    ? -> Used for `Result<T, E>` 
    unwrap -> for both `Result<T, E>` and `Option<T>`
    
    try_to_vec
    try_from_slice
    
    Macro:  
        Function-like -> A Rust macro takes Rust code as input and programmatically expands it into more Rust code
        Attribute-like -> An attribute-like macro takes in a struct and can completely rewrite it.
        custom-derive -> A derive macro augments a struct with additional functions.

    Traits
        trait Speed
        struct Car
        impl Speed for Car
    since the solana is not a object-oriented program -> there won't be any straightforward difference analog between ( public & external ) and ( private & internal )
    you can create a function without pub which is under #[program]

    no direcit inheritence -> indirect way is import the module