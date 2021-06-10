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
