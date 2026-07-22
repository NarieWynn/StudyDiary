Rust code uses `snake case` as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words 

===========================================================
## 1. Mutable Dynamic String
===========================================================
^a71ab6

```rust
let mut string = String::new();
```
- `String::new()` is a function that **allocates/creates** a dynamic string on the Heap.
- `ptr`, `len = 0`, `capacity = 0` at start 0 byte on RAM only has data util `push_str()` 
- This function is in the **prelude** (automatically imported).
- `new()` is an **associated function** of the `String` type (like `static method` in Java/C++), called via `::` syntax.
- Note: An associated function is implemented on a type itself (`String`, `Vec`), not on a specific instance.


```rust
let mut s = String::from("hello");
```
- Must has data at start 
- also can appends a literal to a `String` by using `.push_str("");`
===========================================================
# 2. Receiving Input
===========================================================
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

===========================================================
## 3. References
===========================================================
^05f918
^f04d47
- **Concept:** A reference (`&`) is like a pointer gives a function the **address** of the data, that data is own by some other variable. Therefore, the computer doesn't need to copy the entire data into memory multiple times (saves RAM and improves performance).
- Unlike a pointer, a reference is guaranteed to point to valid value of a particular type for the life of that reference.

- **Mutability Rules:** Like variables, references are **immutable by default**.
    
    - `&guess`: You give the address, but the computer **CANNOT** change the value inside that address (Read-only).
        
    - `&mut guess`: You give the address and grant permission to **CHANGE** the value inside it (Read and Write).

===========================================================
## 4. Overflow handle syntax
===========================================================
^3d04ef

1. `wrapping_add`
```rust
let x: u8 = 255;
let res = x.wrapping_add(1); // res = 0
```
when the value store in variable is overflow (larger or smaller than max/min value). Whether compile in debug mode or with --release flag, this command force the CPU to wrap around and return to 0
2. `checked_add`
```rust
let x: u8 = 255;
let res = x.checked_add(1); // res = None
```
Return the `None` value if there is overflow
3. `overflowing_add`
```rust
let x: u8 = 255;
let (res, is_overflowed) = x.overflowing_add(1); // res = 0, is_overflowed = true
```
Return the value after turning bit 0, and a boolean flag to indicating whether there was overflow
4. `saturaring_add`
```rust
let x: u8 = 255;
let res = x.saturating_add(1); // res = 255
```
Return the maximum or minimum values 

===========================================================
## 5. Declared Data Type
[[C3. Common Programming Concepts#^395b65| See detail]]

===========================================================
## 6. `trim()`
===========================================================

```rust

let mut index = String::new();

io::stdin()
    .read_line(&mut index)
    .expect("Failed to read line");

let index: usize = index
    .trim()
    .parse()
    .expect("Index entered was not a number");
```
`index.trim()` remove space, enter in string (/n)
**Note:** `trim()` only remove space at the head and the end of a string not in the middle "  1   2   " output will be `"1  2"`

===========================================================
## 7. `parse()`
===========================================================
```rust
let mut index = String::new();

io::stdin()
    .read_line(&mut index)
    .expect("Failed to read line");

let index: usize = index
    .trim()
    .parse()
    .expect("Index entered was not a number");
```
`index.parse()` tranform string "3" into a number 3

===========================================================
## 8. if Expression
===========================================================
```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```
^9e6c07
**note:** the condition in this code **must** be a `bool`. If the condition isn't bool, we'll get an error [[C3. Common Programming Concepts#if Expressions|see error here]]
# **Ternary conditional operator**
```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

===========================================================
## 9. Loop in Rust
===========================================================
^a3f890
- Rust has three kinds of loops: `loop, while` , and `for` 
1. `loop`
```rust
fn main() {
    loop {
        println!("again!");
    }
}
```
output 
```bash
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```
- `loop` execute a block of code over and over forever or until you [[English Vocabulary#^b01776|explicitly]] tell it to stop (pressing `ctrl` - `C`)
- Similar to `while(true)` in `C++` and `Java`
- `loop` can also be break by using `break` in code to tell the program where to stop exactly. 
### `continue` 
- This command will tell the program to skip all the code below and start next loop (next iteration).
[[C3. Common Programming Concepts#Disambiguating with Loop Labels|Advanced feature]]
2. `while`
```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```
continue the while loop until the condition is false 
3. `for`
Use `for` loop to loop over the elements of a collection
```rust
fn main() {
	let a = [10, 20, 30, 40, 50];
	
	for element in a {
		println!("the value is: {element}");
	}
}
```
use `while` loop can also bring the same benefit but if you update the array and forgot to change the `index` in `while` loop the code would panic 
- use `for` to decide how many times the loop needs to loop 
increase order
```rust
fn main() {
    for number in 1..4 {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```
decrease order
```rust
fn main() {
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```

===========================================================
## 10. 
===========================================================