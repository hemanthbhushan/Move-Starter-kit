# Rust Learning Resources

1. [**Rust Book**](https://doc.rust-lang.org/book/):
   - Comprehensive resource covering Rust programming language.
   - Recommended to complete all chapters, with a particular emphasis up to Chapter 11.

# Move Language

2. [**Move Lang Book**](https://move-book.com/index.html):
   - In-depth guide for Move language with examples.

3. **Gas Optimization on Move Aptos:**
   - [Research Paper](https://www.eecg.utoronto.ca/~veneris/brains23.pdf)
   - [Source Code](https://github.com/Veneris-Group/Move-Gas-Optimization-Patterns/tree/main/source)

4. **Aptos Move VM YouTube Video:**
   - [Aptos Move VM](https://www.youtube.com/watch?v=1DGj0SQa8zQ&t=39s)

# Additional Reading

5. **Medium Articles:**
   - [Replay Aptos Tutorial Episode 1: Create Things](https://medium.com/@magnum6/replay-aptos-tutorial-episode-1-create-things-90920fcdf409)
   - [Continuation of Aptos Tutorial](https://medium.com/code-community-command/were-picking-up-where-we-left-off-at-the-last-episode-so-if-this-is-your-first-time-here-check-394ddb8950f0)
   - [Aptos Tutorial Episode 3: Deploy Things (and, boy, did those things just get easier)](https://medium.com/code-community-command/aptos-tutorial-episode-3-deploy-things-94eb973a7a51)
   - [Aptos Key Rotation | Learning Move 0x02](https://noncegeek.medium.com/aptos-key-rotation-learning-move-0x02-809053f29aff)
   - [Accounts in Aptos](https://medium.com/@martian-wallet/accounts-in-aptos-1ecc3f0b1213)

6. **Move Whitepaper:**
   - [Whitepaper Deep Dive into Move - Facebook Libra Blockchain's New Programming Language](https://medium.com/coinmonks/whitepaper-deep-dive-move-facebook-libra-blockchains-new-programming-language-7dbd5b242c2b)

7. **Move VM Documentation:**
   - [Move VM Documentation](https://docs.dfinance.co/move_vm)

# Video Resources

8. **YouTube Videos:**
   - [MOVE APTOS 01: MOVE and APTOS Introduction. MOVE contract code explanation](https://youtu.be/YaKmh8G4KVU?feature=shared)
   - [MOVE contract deployment and calling function from the deployed contract](https://youtu.be/He3erI1ijpU?feature=shared)
9. **APTOS REFERENCES:**
   - [move-stdlib](https://aptos.dev/reference/move/?branch=mainnet&branch=mainnet&page=aptos-framework/doc/guid.md)
   - [Marketplace_Aptos_move GitHub Repository](https://github.com/hemanthbhushan/Marketplace_Aptos_move)
10 Can deploy the contracts directly from the module like a factory contract 
step 1 : 
go into the module and run the command 
```aptos move compile --save-metadata```
then it creates the file .mv for the code and bcs for the matadata 
then run the below code to create the serilised data to pass in to the 
publish_package_txn() function which deploys the contracts 
make the address at your deploying and the and address you set in the toml are same
// cargo run --release -- "projects/dexlyn/dexlyn_swap/dexlyn_swap_lp/build/DexlynswapLP/package-metadata.bcs" "projects/dexlyn/dexlyn_swap/dexlyn_swap_lp/build/DexlynswapLP/bytecode_modules/lp_coin.mv"

use std::fs::File;
use std::io::{self, Read};
use std::path::PathBuf;
use std::env;
use dirs::home_dir;
use hex::encode;

fn main() -> io::Result<()> {
    // Collect command-line arguments
    let args: Vec<String> = env::args().collect();

    // Check if at least one file path is provided
    if args.len() < 2 {
        eprintln!("Usage: {} <relative_path1> <relative_path2> ...", args[0]);
        return Err(io::Error::new(io::ErrorKind::InvalidInput, "No file paths provided"));
    }

    // Get the user's home directory
    let home_dir = match home_dir() {
        Some(home) => home,
        None => {
            eprintln!("Could not determine home directory");
            return Err(io::Error::new(io::ErrorKind::NotFound, "Home directory not found"));
        }
    };

    // Iterate over each provided file path
    for relative_path in &args[1..] {
        // Construct the full path
        let mut path = PathBuf::from(&home_dir);
        path.push(relative_path);

        println!("Processing file: {:?}", path);

        // Step 1: Read the file into a byte array
        let mut file = match File::open(&path) {
            Ok(f) => f,
            Err(e) => {
                eprintln!("Failed to open file {:?}: {}", path, e);
                continue;
            }
        };
        let mut buffer = Vec::new();
        file.read_to_end(&mut buffer)?;

        // Step 2: Convert the byte array into a hex string
        let hex_string = encode(buffer);

        // Print the hex string or use it as needed
        println!("{}", hex_string);
    }

    Ok(())
}

