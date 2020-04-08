# rust_summary
Summary of https://doc.rust-lang.org/stable/book


## [3. Common Programming Concepts](https://doc.rust-lang.org/stable/book/ch03-00-common-programming-concepts.html)

### [3.1. Variables and Mutability](https://doc.rust-lang.org/stable/book/ch03-01-variables-and-mutability.html)

```
Variable immutables : let x = 5; ( no change, let x = x+1 is not possible ) << shadowing
Variable mutable: let mut x = 5; ( you can add x = x+1 )
Const: const MAX_POINTS: u32 = 100_000;
```


### [3.2. Data Types](https://doc.rust-lang.org/stable/book/ch03-02-data-types.html)

#### Scalar types

##### Integer types

| Length | Signed | Unsigned |
| --- | --- | --- |
| 8-bit | i8 | u8 |
| 16-bit | i16 | u16 |
| 32-bit | i32 | u32 |
| 64-bit | i64	| u64 |
| 128-bit | i128 | u128 |
| arch | isize | usize |

##### Integer literals

| Number literals	| Example |
| --- | --- |
| Decimal	| 98_222 |
| Hex	| 0xff |
| Octal	| 0o77 |
| Binary	| 0b1111_0000 |
| Byte (u8 only)	| b'A' |

##### Floating-Point types

```
Default: let x = 2.0; // f64
(The default type is f64 because on modern CPUs it’s roughly the same speed as f32 but is capable of more precision. )
Explicit: let y: f32 = 3.0; // f32
```

##### Numeric operations

```
+ , - , * , / , %
```

##### Boolean type

```
Implicit: let t = true;
Explicit: let f: bool = false;
```

##### Character type

Rust’s char type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid charvalues in Rust. Unicode Scalar Values range from U+0000 to U+D7FF and U+E000 to U+10FFFF inclusive. However, a “character” isn’t really a concept in Unicode, so your human intuition for what a “character” is may not match up with what a char is in Rust.

```
let c = 'z';
let z = 'ℤ';
let heart_eyed_cat = '😻';
```

#### Compount types

##### Tuple type

Collection with variety of types and fixed length, accessing by .index or using pattern matching

``` 
let x: (i32, f64, u8) = (500, 6.4, 1);

let five_hundred = x.0;

let six_point_four = x.1;

let one = x.2;
let (a, b, c) = x;
```

##### Array type

Collection but same type and fixed length, accessing by [index]

```
Declarations:
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];
let a = [3; 5];  ( let a = [3, 3, 3, 3, 3]; )
```

### [3.3. Functions](https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html)

function argument declarative fn plus_one(x: i32)
Statement does not return value and expression needs to remove semicolon last line to return that value
function return i32, argument i32 x, expression inside let statement, returning y+1:

```
fn plus_one(x: i32) -> i32 {
    let y = {
        let x = 3;
        x + 1
    };
    y + 1
}
```

### [3.4. Comments](https://doc.rust-lang.org/stable/book/ch03-03-how-functions-work.html)

```
// Comment here
```

### [3.5. Control Flow](https://doc.rust-lang.org/stable/book/ch03-05-control-flow.html)

#### if Expressions

**if / else / else if**

```
let number = 3;

if number < 5 {
    println!("condition was true");
} else if number > 10 {
    println!("condition was false");
} else {
    Println!(“condition in the middle”);
}
```
#### Using if in a let statement

Return should have the same format!!! 5 and 6 are number, it is not possible return 5 and “six”, but “five” and “six” would be correct

```
let number = if condition {
    5
} else {
    6
};
```

#### Repetitions with loop

##### Repeting with loop

Loop: executes block until an explicity end
Returning a value with loop in let statement ( break, continue )

```
let result = loop {
  counter += 1;
  if counter == 10 {
    break counter * 2;
  }
};
```

##### Conditional loops with while

```
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

##### Looping through a collection with for

```
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```
**Range**

```
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

## 4_Ownership

### 4.1 What is it?

#### Rules

> Each value in Rust has a variable that’s called its owner
> There can only be one owner at a time
> When the owner goes out of scope, the value will be dropped

#### Stack and Heap

Stack: Last IN, first OUT ( fixed size )
Heap: allocation with reference or pointer ( unknown size )

#### Variable Scope

> when variable comes into scope { , it is said
> It remains valid until it goes out of scope }

#### String type

String literal: we know the content, it is hardcode, fast and efficient but immutable ( compiled )
String mutable ( let mut s = String::from("hello”); ) unknown content, we need to allocate it on heap

#### Memory and Allocation

The drop ( free memory of var ) is done at the end of scope } ( We cannot drop twice, early or forget )
> The memory must be requested from OS at runtime
> We need a way to returning the memory
Drop function Rust is called Resource Acquisition Is Initialization (RAII) in C++

#### Ways Variables and Data Interact:

##### Move

```
let s1 = String::from("hello");
let s2 = s1;
```
s1 is on stack (pointer,len,capacity) and heap ( data content, memory). We only copy to s2 the stack block ( pointer, length, capacty ), but we cannot drop s1 and s2 ( It would be twice ). Solution is s1 cannot be used after move.

##### Clone

```
let s1 = String::from("hello");
let s2 = s1.clone();
```
Here we are copying stack block and heap block, so we don’t have the drop problem

##### Copy

```
let x = 5;
let y = x;
```
There’s no problem here, some data type are in stack so we can copy them.
Here are some of the types that are Copy:

All the integer types, such as u32.
The Boolean type, bool, with values true and false.
All the floating point types, such as f64.
The character type, char.
Tuples, if they only contain types that are also Copy. For example, (i32, i32) is Copy, but (i32, String) is not.

#### Ownership and Functions

```
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

#### Return Values and Scope

```
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 goes out of scope but was
  // moved, so nothing happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("hello"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// takes_and_gives_back will take a String and return one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into
                                                      // scope

    a_string  // a_string is returned and moves out to the calling function
}
```
### 4.2 References and Borrowing
You can pass the refence ( pointer of content ) with & and ownership is not sent
```
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```
NOTE: * is dereference opossite of & reference

We call have references as function paramenters borrowing.

#### Mutable References

NOTE: &String is not mutable
Changing &String to:
```
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}

```
But mutable references have one big restriction: you can have only one mutable reference to a particular piece of data in a particular scope. This code will fail:

```
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s; <<<<<<<<<<<< NOT POSSIBLE!!!

println!("{}, {}", r1, r2);
```

The benefit of having this restriction is that Rust can prevent data races at compile time. A data race is similar to a race condition and happens when these three behaviors occur:

Two or more pointers access the same data at the same time.
At least one of the pointers is being used to write to the data.
There’s no mechanism being used to synchronize access to the data.

You cannot mix immutable and muttables:
```
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM

println!("{}, {}, and {}", r1, r2, r3);
```
But if variables are out of the scrop is allowed:
```
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
println!("{} and {}", r1, r2);
// r1 and r2 are no longer used after this point

let r3 = &mut s; // no problem
println!("{}", r3);
```
### Dangling References
Is not possible send a reference from function to outside ( you loose the ownership )
```
fn main() {
    let reference_to_nothing = dangle();
}
fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Danger!
```
Solution could be return a String with ownership ( stack and heap block )
### Rules of references
Let’s recap what we’ve discussed about references:

At any given time, you can have either one mutable reference or any number of immutable references.
References must always be valid.

## 4.3 The Slice Type
Slice type does not have ownership
### String slices
let s = String::from("hello world");
```
let hello = &s[0..5];
let world = &s[6..11];
```
We can create slices using a range within brackets by specifying [starting_index..ending_index]
```
let s = String::from("hello");

let len = s.len();

let slice = &s[0..len]; >> whole content
let slice = &s[..];     >> equal than above
```
### String literals are slices
let s = "Hello, world!”; >>> &str
### String Slices as Paramaters
Good:
```
fn first_word(s: &String) -> &str {
```
Better:
```
fn first_word(s: &str) -> &str {
```
Because String could be str

Example:
```
fn main() {
    let my_string = String::from("hello world");

    // first_word works on slices of `String`s
    let word = first_word(&my_string[..]);

    let my_string_literal = "hello world";

    // first_word works on slices of string literals
    let word = first_word(&my_string_literal[..]);

    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal);
}
```
### Other slices

```
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];
```
