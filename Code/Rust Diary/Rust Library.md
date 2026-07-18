## 1. Input/Output 

^963aeb

```rust
use std::io; // Brings the I/O library into the current scope
```
- **Purpose:** Provides essential traits, functions, and structures to handle system Input and Output operations.
    
- **Core Functionality in Chapter 2:**
    
    - Manages the standard input stream from the keyboard (`stdin`).
        
    - Provides methods to read user input line-by-line (`read_line`).
        
    - Handles potential operational errors via the `Result` type and error handling methods (such as `.expect()`).
        
- **Note:** While part of the Rust Standard Library (`std`), `io` is **not** included in the `prelude`. Therefore, it must be explicitly imported using the `use` keyword at the top of the file.
# Some syntax of this library
- [[Rust Syntax#^fc4933|io::stdin().read_line(&mut guess)]] 
- 
