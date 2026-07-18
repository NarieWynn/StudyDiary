## 1. Mutable Dynamic String

^a71ab6

```rust
let mut string = String::new();
```
- `String::new()` is a function that **allocates/creates** a dynamic string on the Heap.
- This function is in the **prelude** (automatically imported).
- `new()` is an **associated function** of the `String` type (like `static method` in Java/C++), called via `::` syntax.
- Note: An associated function is implemented on a type itself (`String`, `Vec`), not on a specific instance.
# 2. Receiving Input

^fc4933

```rust
io::stdin() // call the function stdin from io module
	.read_line(&mut guess) // call the method in io library
```
- `io::stdin()` is a function from `io` module that returns an instance of standard input (`Stdin`).
- `read_line()` is a **method** of that `Stdin` instance.
- `&mut guess` is a mutable reference parameter, telling `read_line` which string to store the input into.
**Note:** To use this function first declare [[Rust Library#^963aeb| Input/Output Library]]
 If `std::io` is not imported via `use`, you can still use it by writing full path: 
```rust
std::io::stdin
```
- **Mechanism:** `read_line` takes whatever the user types and **appends** into the string without overwriting. (Imagine a bucket: new input is added to the bottom, it doesn't empty the bucket first).
## 3. References

^f04d47

- **Concept:** A reference (`&`) gives a function the **address** of the data. Therefore, the computer doesn't need to copy the entire data into memory multiple times (saves RAM and improves performance).
    
- **Mutability Rules:** Like variables, references are **immutable by default**.
    
    - `&guess`: You give the address, but the computer **CANNOT** change the value inside that address (Read-only).
        
    - `&mut guess`: You give the address and grant permission to **CHANGE** the value inside it (Read and Write).
