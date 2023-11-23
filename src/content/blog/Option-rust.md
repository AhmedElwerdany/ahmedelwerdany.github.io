---
author: Ahmed Elwerdany
pubDatetime: 2023-12-23T15:57:52.737Z
title: Adding Null to Rust | Implementing Option From Scratch
postSlug: implementing-option-from-scratch
featured: true
ogImage: /assets/posts/null-rust/image1.jpeg
tags:
  - rust
  - from scratch
description: Explore the depths of Rust’s type system as we add null handling capabilities by implementing the ‘Option’ enum from scratch. Discover how to elegantly handle the absence of a value and safely unwrap the ‘Option’ enum in Rust. This article provides a step-by-step guide to understanding and creating your own version of the ‘Option’ enum, complete with code examples and explanations.
---

![creating the Option enum from scratch](/assets/posts/null-rust/image1.jpeg)
Welcome, code adventurers! Today, we're delving into Rust's `Option` enum. We're not just studying it; we're recreating it. We'll rename `None` to `Null`. This isn't merely an enum; it's a tool that handles the absence of a value with elegance and safety. So, join us as we decode mysteries and unravel enigmas in the shadows of code. Let's light the path to knowledge and defeat boredom. Onward!

We'll start with a few examples of `Option`, then craft our own version, which we'll call `Choice`.

## What is Option?

At its core, `Option` is an __enum__. In Rust, an enum can be defined as follows:

```rust
enum Creature {
	Human,
	Animal
}
```

Enum variant can hold a value:

```rust
enum Creature {
	Human(String),
	Animal
}
```

So, if we convert that to `Option`, it would look like this:

```rust
enum Option {
	Some(String),
	None
}
```

However, we don't want to limit ourselves to `String`s only. We should let the user decide which type to use. We can achieve this with Generics.

```rust
enum Option<T> {
	Some(T),
	None
}

let number = Option::Some(100);

matches!(number, 100); // this will not work
```

But we need to be able to use the `100`. The variable `number` is still equal to the `enum`, not the value inside of it.  
For that, we need to implement the `unwrap` method.

## Unwrapping the Option

First, let's rename our enum to `Choice` instead of `Option` to avoid confusion with Rust's built-in `Option`.

### Checking Which Value is Picked in an Enum

A straightforward way to solve this is by matching on `number`, getting the value, and assigning it to another variable:

```rust
enum Choice<T> {
	Some(T),
	Null
}

fn return_some() -> Opt<i32> {
	Choice::Some(100)
}

let number = return_some();
let mut value = 0;
match number {
	Choice::Some(v) => {value = v},
	_ => {}
};
println!("{}", value); // 100
```

We can simplify this better by using `if let`:

```rust
enum Choice<T> {
	Some(T),
	Null
}

fn return_some() -> Choice<i32> {
	Choice::Some(100)
}

let mut number = 0;
if let Choice::Some(v) = return_some() {
	number = v
};
println!("{}", number); // 100
```

But a better way is to add a method `get_value`, something like this:

```rust
let number = return_some().get_value();
```

In `Option`, Rust calls it `unwrap`. So let's do exactly that but replacing the name with `get_value`:

```rust
impl<T> Choice<T> {
	fn get_value(self) -> T { // it will return the same type of the "Choice"
	 match self {
		Self::Some(value) => value,
		Self::Null => panic!("called unwrap on Null Type") 
	 }
	}
}
```

Now let's use it:

```rust
	let number = get_some().get_value();
	println!("{}", number); // 100
```

## Handling the Null Variant: A Beacon in the Shadows
In the labyrinth of code, encountering the `None` variant can often feel like stumbling upon a dead end. But fear not, for Rust provides us with a torch to illuminate these dark corners: the `expect` method. This method can be invoked on an `Option` like so:

```rust
	let number = get_none().expect("Error");
```

Notice that the function is now called `get_none`:

```rust
fn get_none() -> Option<i32> {
	None
}
```

The program will panic, and we will get an error:

```console
thread 'main' panicked at src/main.rs:2:32:
error
```

### Illuminating the Path: Adding Handling for the Null Variant to Our Enum
To further light our path, let’s add a method to our enum to handle the `Null` variant. We’ll call this method `if_null`:

```rust
impl<T> Choice<T> {
	fn if_null(self, message: &str) -> T {
		match self {
			Self::Some(value) => value,
			Self::Null => panic!("{}", message)
		}
	}
	fn get_value(self) -> T { // it will return the same type of the Option
	 match self {
		Self::Some(value) => value,
		Self::Null => panic!("called unwrap on None Type") 
	 }
	}
}
```

Here's the complete code:

```rust
enum Choice<T> {
    Some(T),
    Null,
}

impl<T> Choice<T> {
    fn if_null(self, message: &str) -> T {
        match self {
            Self::Some(value) => value,
            Self::Null => panic!("{}", message),
        }
    }
    fn get_value(self) -> T {
        match self {
            Self::Some(value) => value,
            Self::Null => panic!("called unwrap on Null Variant"),
        }
    }
}


fn main() {
    get_some_example();
    get_null_example()
}

fn get_some() -> Choice<i32> {
    Choice::Some(100)
}

fn get_null() -> Choice<i32> {
    Choice::Null
}

fn get_some_example() {
    let number = get_some().get_value();
    println!("{}", number)
}

fn get_null_example() {
    let number = get_null().if_error("error");
    println!("{}", number)
}
```

And there you have it! We've implemented our own version of the `Option` enum in Rust `Choice`. Remember, the power of code is in your hands. Keep hunting, and may your path be illuminated by knowledge and skill. Until next time!


##### references:
- [what is an Option and enum in Rust](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html#the-option-enum-and-its-advantages-over-null-values)
- [the real implementation of the Option enum in Rust](https://github.com/rust-lang/rust/blob/91f7f266ce973a08e6e700f9ce815f7b61d347e9/library/core/src/option.rs#L570)