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
