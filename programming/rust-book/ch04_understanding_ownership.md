# Understanding Ownership

Rust's most unique feature, allowing it to make memory safety guarantees with no garbage collection.

## What Is Ownership?
All programs have to manage how they use memory while running.
- Some languages have a garbage collector that checks for unused memory to free it
- Other languages leave it to the programmer to allocate and free memory.

Rust instead manages memory through a system of ownership, which the compiler checks at compile
time. The compiler essentially does the "manual management" for you, by using the ownership system
to figure out when memory can be freed.

This chapter will focus on ownership by looking at strings.

### The Stack and the Heap
The stack is a last in, first out system to store data in memory. Think of `git stash`. All data
stored on the stack must have a known, fixed size - if the size is unknown *at compile time* or can
change, the data must be stored on the heap instead.

To store data on the heap, the memory allocator finds space in memory that is large enough for the
data, and returns a pointer to that memory location. This is called "allocating on the heap", or
just "allocating". (Because the pointer is a fixed size, the pointer can be pushed onto the stack,
but to get the data itself you have to follow the pointer to the heap.)

Pushing to the stack is faster than allocating on the heap:
- Allocator doesn't need to find where to store data, since it's always pushed to the top of the
  stack.
- When allocating you need to find enough space for the data you are trying to store, and then you
  (the operating system?) must do some work to prepare the next allocation (e.g. marking that area
  as used memory, I assume).

Accessing data on the stack is faster than on the heap, because to get data from the stack you have
to follow a pointer.

When you call a function, the values passed into the function (including pointers) and the
function's local variables are pushed onto the stack; when the function is over, those values are
popped off the stack.

Ownership is all about managing data on the heap.

### Ownership Rules
- **Each value in rust has a variable called its owner**
- **There can only be one owner at a time**
- **When the owner goes out of scope, the value will be dropped.**

### Variable Scope
A scope is the range inside the program where an item is valid.

Example:
```rust
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward

    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

This should be familiar from other languages.

### The `String` Type
String literals, like `"this"`, are string values hardcoded into the program. However they have
limitations: they are immutable, and also we may want to initialise a value that we don't know at
compile time (user input, etc).

Rust has a second string type, `String`, to cover these limitations. *This type is allocated on the
heap*, so it's able to store amount of text that is unknown at compile time. Strings can be created
from string literals and mutated:
```rust
let mut s = String::from("hello");

s.push_str(", world!"); // push_str() appends a literal to a String

println!("{}", s); // This will print `hello, world!`
```

### Memory and Allocation
Why can `String`s be mutated while literals can't?

The content of a string literal is hardcoded into the executable itself, which is why it can't
mutate - we can't store every single variation of a string in finite space...

In order to have data that can change and grow, we need to allocate `String` types on the heap. That
means we need a way of returning the allocated memory to the allocator once we're finished using it.
