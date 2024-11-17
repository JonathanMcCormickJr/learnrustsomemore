# LearnRustSomeMore

By Jonathan McCormick, Jr. 

**A notespace for me as I walk through** 
* ["A Gentle Introduction To Rust"](https://stevedonovan.github.io/rust-gentle-intro/readme.html#a-gentle-introduction-to-rust) by Steve Donovan ([GitHub link](https://github.com/stevedonovan/gentle-intro/blob/master/src/readme.md)), 
* ["The Book"](https://doc.rust-lang.org/stable/book/), and 
* ["Programming Rust, 2nd Edition"](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/)  by Jim Blandy, Jason Orendorff, Leonora F . S. Tindall


## 2024-11-02
Rust's compiler is your friend. 

### Some external crates that are interesting: 
`diesel`: ORM and query builder for PostgreSQL, SQLite, and MySQL. 
`rand`: Random number generators. 
`tokio`: an event-driven, non-blocking I/O platform for writing asynchronous applications. 

### Borrowing

For some reason this compiles...

``` rust 
fn main() {
    let var_x = "My string";
    println!("var_x = {}", var_x);

    let var_y = var_x;
    println!("var_y = {}", var_y);

    println!("var_x = {}", var_x);

}
```
``` 
$ cargo run
   Compiling learnrustsomemore v0.1.0 (/home/jonathan/projects/learnrustsomemore)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/learnrustsomemore`
var_x = My string
var_y = My string
var_x = My string
```

... but this does not:
``` rust 
fn main() {
    let var_x = "My string".to_string();
    println!("var_x = {}", var_x);

    let var_y = var_x;
    println!("var_y = {}", var_y);

    println!("var_x = {}", var_x);

}
```
```
$ cargo run
   Compiling learnrustsomemore v0.1.0 (/home/jonathan/projects/learnrustsomemore)
error[E0382]: borrow of moved value: `var_x`
 --> src/main.rs:8:28
  |
2 |     let var_x = "My string".to_string();
  |         ----- move occurs because `var_x` has type `String`, which does not implement the `Copy` trait
...
5 |     let var_y = var_x;
  |                 ----- value moved here
...
8 |     println!("var_x = {}", var_x);
  |                            ^^^^^ value borrowed here after move
  |
  = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider cloning the value if the performance cost is acceptable
  |
5 |     let var_y = var_x.clone();
  |                      ++++++++

For more information about this error, try `rustc --explain E0382`.
error: could not compile `learnrustsomemore` (bin "learnrustsomemore") due to 1 previous error
```

[Update from 2024-11-17: This is due to the first example of `let var_x = "My string";` having a value of `&str`, which is Copy. The second example uses the `.to_string()` method, resulting in a value of the owned type `String`, which is not Copy.]

## 2024-11-03
Who knew working with strings could be this complicated? Lol. 

## 2024-11-17

[This code has way too many comments and is presented regardless.]
``` rust 
fn main() {                     // Start of the main function. 
    let mut x = vec![1, 2, 3];  // Declare a mutable vector of i32 values.
    for mut num in &mut x {     // Create a for loop which borrows a mutable
                                // reference to our vector x. 
        *num += 1;              // For each dereferenced item in x, increment it by 1. 
    }                           // Ending of our for loop's scope. 
    println!("{:?}", x);        // Print our results. 
}                               // Ending of the main function. 
```

