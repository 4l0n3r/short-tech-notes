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

supports only fixed array, for dynamic we have vectors
Rust does not have try catch.

copy types -> fixed size
Ownership:
    many read references / one write reference. not both in same scope.
? -> Used for `Result<T, E>` 
unwrap -> for both `Result<T, E>` and `Option<T>`

try_to_vec
try_from_slice
