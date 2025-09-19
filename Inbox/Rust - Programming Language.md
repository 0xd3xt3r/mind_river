---
tags:
  - "#type/knowledge"
status:
  - todo
created-date: 2024-12-15
up: "[[Knowledge Base MoC]]"
related: 
summary: Learning Rust related programming
---

Book Recommendation : The Rust Programming Language, 2nd Edition (NoStarchPress)


# References and Borrowing

- Temporally taking ownership of object is called Referencing. **Drop** function is not called on object with are references.
- Just as variables are immutable by default, so are references. We’re not allowed to modify something we have a reference to.
- We can make references mutable by "mut" syntax.
- But mutable references have one big restriction: you can have only one mutable reference to a particular piece of data at a time. The restriction preventing multiple mutable references to the same data at the same time allows for mutation but in a very controlled fashion.
- The benefit of having this restriction is that Rust can prevent data races at compile time. A _data race_ is similar to a race condition and happens when these three behaviors occur:
	-   Two or more pointers access the same data at the same time.
	-   At least one of the pointers is being used to write to the data.
	-   There’s no mechanism being used to synchronize access to the data.
- We also cannot have a mutable reference while we have an immutable one. Users of an immutable reference don’t expect the values to suddenly change out from under them! However, multiple immutable references are okay because no one who is just reading the data has the ability to affect anyone else’s reading of the data.
- erroneously create a _dangling pointer_, a pointer that references a location in memory that may have been given to someone else, by freeing some memory while preserving a pointer to that memory. In Rust, by contrast, the compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

## Traits

_Traits_ are Rust’s take on interfaces or abstract base classes. At first, they look just like interfaces in Java or C#.

```rust
pub trait Summary {
	fn summarize(&self) -> String; 
}
```

## Generics

- _Generics_ are the other flavor of polymorphism in Rust. Like a C++ template, a generic function or type can be used with values of many different types:

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };
    println!("p.x = {}", p.x());
}
```

## Iterator

-   `iter()` iterates over the items by reference
-   `iter_mut()` iterates over the items, giving a mutable reference to each item
-   `into_iter()` iterates over the items, moving them into the new scope


## Lifetime

- Lifetime annotations don’t change how long any of the references live. Rather, they describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes
- Lifetime annotations are meant to tell Rust how generic lifetime parameters of multiple references relate to each other.
- Syntax
	```rust
	fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
	    if x.len() > y.len() {
	        x
	    } else {
	        y
	    }
	}
	```
- When we specify the lifetime parameters in this function signature, we’re not changing the lifetimes of any values passed in or returned. Rather, we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints.
- When annotating lifetimes in functions, the *annotations go in the function signature*, not in the function body. The lifetime annotations become part of the contract of the function, much like the types in the signature. Having function signatures contain the lifetime contract means the analysis the Rust compiler does can be simpler.
- When *returning a reference* from a function, the lifetime parameter for the return type needs to match the lifetime parameter for one of the parameters. If the reference returned does not refer to one of the parameters, it must refer to a value created within this function. However, this would be a dangling reference because the value will go out of scope at the end of the function.
- Lifetime Annotations in Struct Definitions
	- Structs can hold owned types. We can define structs to hold references, but in that case we would need to add a lifetime annotation on every reference in the structs definition.
		```rust
		struct ImportantExcerpt<'a> {
		    part: &'a str,
		}
		
		fn main() {
		    let novel = String::from("Call me Ishmael. Some years ago...");
		    let first_sentence = novel.split('.').next()
			    .expect("Could not find a '.'");
		    let i = ImportantExcerpt {
		        part: first_sentence,
		    };
		}
		```
	- This struct has the single field `part` that holds a string slice, which is a reference. As with generic data types, we declare the name of the generic lifetime parameter inside angle brackets after the name of the struct so we can use the lifetime parameter in the body of the struct definition. This annotation means an instance of `ImportantExcerpt` can’t outlive the reference it holds in its `part` field.
- **Lifetime Elision Rules**: Lifetimes on function or method parameters are called _input lifetimes_, and lifetimes on return values are called _output lifetimes_. The compiler uses three rules to figure out the lifetimes of the references when there aren’t explicit annotations.
	1. The first rule is that the compiler assigns a lifetime parameter to each parameter that’s a *reference*.
		1. In other words, a function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`; a function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`; and so on.
	2. The second rule is that, if there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: `fn foo<'a>(x: &'a i32) -> &'a i32`.
	3. The third rule is that, if there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters. This third rule makes methods much nicer to read and write because fewer symbols are necessary. The third rule really only applies in method signatures
1. **The Static Lifetime** : One special lifetime we need to discuss is 'static, which denotes that the affected reference can live for the entire duration of the program. All string literals have the 'static lifetime, which we can annotate as follows: 
	```rust
	let s: &'static str = "I have a static lifetime.";
	```