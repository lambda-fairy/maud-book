# Getting started

Example
-------

Here's a minimal example of Maud in action:

```rust
#![feature(plugin)]

extern crate maud;
#[plugin] #[no_link] extern crate maud_macros;

fn main() {
    let name = "Lyra";
    let markup = html! {
        p { "Hi, " $name "!" }
    };
    assert_eq!(markup.to_string(), "<p>Hi, Lyra!</p>");
}
```

Getting started
---------------

First off, add `maud` and `maud_macros` to your `Cargo.toml`:

```toml
[dependencies]
# ...
maud = "*"
maud_macros = "*"
```

`maud` contains various types and functions used by the generated code. `maud_macros` provides the `html!` macro itself. Both are essential to using the library.
