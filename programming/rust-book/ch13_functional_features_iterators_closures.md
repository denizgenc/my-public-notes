# Functional Language Features: Iterators and Closures
Functional programming often involves using functions as values that you pass into other functions
to return new functions, etc.

Rust, while not being a "functional" language in a strict sense, has a few features from functional
languages that this chapter looks in to. These include:
- Closures - "a function-like construct you can store in a variable"
- Iterators
- How to use these features to improve I/O

There are other features, such as pattern matching and enums, which are inspired by functional
languages, but they're covered in other chapters.

Closures and iterators are an important part of writing **idiomatic and fast** Rust code, so it's
important to understand and use them.

## Closures: Anonymous Functions that Can Capture Their Environment
The syntax for closures is one of the following:
```rust
let add_one_v1 = |x: u32| -> u32 { x + 1 };
let add_one_v2 = |x|             { x + 1 };
let add_one_v3 = |x|               x + 1  ;
```

Unlike functions, closures don't require a type annotation for their parameters or return types.
It's automatically inferred from the first time the closure is called. However, this means that you
can't call them with different types. This causes a compiler error:
```rust
let example_closure = |x| x;

let s = example_closure(String::from("hello"));
let n = example_closure(5);
```

The reason we can do this is because closures aren't exposed to other parts of the program, or even
other developers (if it's a library) - they're used within functions themselves, so you're not
making a strict contract that you have to fulfil, kind of.

## Storing Closures Using Generic Parameters and the `Fn` Traits
Imagine we have the following function, that conditionally needs to call an expensive calculation
function:

```rust
fn generate_workout(intensity: u32, random_number: u32) {
    if intensity < 25 {
        println!(
            "Today, do {} pushups!",
            expensive_calculation(intensity)
        );
        println!(
            "Next, do {} situps!",
            expensive_calculation(intensity)
        );
    } else {
        if random_number == 3 {
            println!("Take a break today! Remember to stay hydrated!");
        } else {
            println!(
                "Today, run for {} minutes!",
                expensive_calculation(intensity)
            );
        }
    }
}
```

We can see that if `intensity` is less than 25, we have to do the expensive calculation twice. And
in general there's a bunch of different references to it that can make it difficult to change
`generate_workout()` if we decide to change how or when we call the `expensive_calculation()`. So we
want to **refactor this code so we only call `expensive_calculation()` once**, and that we don't
call it if the result isn't needed.

For example, if we try storing the result of `expensive_calculation()` outside the if statements, it
will still be called when the function decides the user should take a break. Not ideal.

One way to solve this problem is to use closures; specifically, to store a closure in a struct that
will only call the closure when a new parameter is passed into it, and return the value of that call
whenever the same call is made later. This is "memoization" or "lazy evaluation".

Note that structs require types for every field - a closure can be represented by the traits `Fn`,
`FnMut`, or `FnOnce`. Here's an example that we'll use:
```rust
struct Cacher<T>
where
    T: Fn(u32) -> u32,
{
    calculation: T,
    value: Option<u32>, // This is wrong! It should hold a hash map
}
```

Here, `value` will be `None` until code asks the `Cacher` for result of the closure, after which it
will be `Some` value. We implement this behaviour like so:
```rust
impl<T> Cacher<T>
where
    T: Fn(u32) -> u32,
{
    fn new(calculation: T) -> Cacher<T> {
        Cacher {
            calculation,
            value: None,
        }
    }

    fn value(&mut self, arg: u32) -> u32 {
        match self.value {
            Some(v) => v, // Wrong! Always gives us the same result after the first call to value()
            None => {
                let v = (self.calculation)(arg);
                self.value = Some(v);
                v
            }
        }
    }
}
```

TODO: This code doesn't do what you expect it to do. `Cacher` will hold on to the same `value` after
the first call to `Cacher.value()`, so even if we pass in a different parameter to `value()`, it
will give us the same result. The Rust Book admits this... To be fair, it *does* work for the
example we're looking into, where we have a function that only ever uses the same parameter for the
function.

## Capturing the Environment with Closures
Closures can do something functions cannot: **they can captures their environment and access
variables from the scope in which they're defined**. See:
```rust
fn main() {
  let x = 4;

  let equal_to_x = |z| z == x;

  let y = 4;

  assert!(equal_to_x(y));
}
```

Even though `x` is not one of the parameters of `equal_to_x`, it's still able to be used by the
closure. This is clearly not possible in functions. A closure can take these values in one of three
ways:
- `FnOnce` consumes the variables it captures from the environment. As such, it can only be called
  once.
- `FnMut` mutably borrows the captured values - thus, it can change the environment.
- `Fn` immutably borrows the captured values.

Rust infers which trait to use based on how you use the closure in the code. Note that all closures
implement `FnOnce`, since they can all be called at least once.
