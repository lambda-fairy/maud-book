# Getting started

Like most Rust packages, Maud uses [Cargo] for dependency management.

[Cargo]: https://crates.io/

First off, add `maud` and `maud_macros` to your `Cargo.toml`:

```toml
[dependencies]
# ...
maud = "*"
maud_macros = "*"
```

The `maud` crate contains types and functions used by the generated code, whereas `maud_macros` provides the `html!` macro itself. Both are essential to using the library.

Now save the following to `src/main.rs`:

```rust
#![feature(plugin)]

extern crate maud;
#[plugin] #[no_link] extern crate maud_macros;

fn main() {
    let name = "Lyra";
    let markup = html! {
        p { "Hi, " $name "!" }
    };
    println!("{}", markup.to_string());
}
```

Run this program with `cargo run`, and you'll (hopefully) get the following:

```
<p>Hi, Lyra!</p>
```
