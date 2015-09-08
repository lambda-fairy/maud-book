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

The `maud` crate exports types and functions used by the generated code, whereas `maud_macros` provides the `html!` macro itself. Both are essential to using the library.

Now save the following to `src/main.rs`:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate maud;

use std::io;

fn main() {
    let name = "Lyra";
    write_html!(io::stdout(), {
        p { "Hi, " $name "!" }
    }).unwrap();
}
```

Run this program with `cargo run`, and you'll (hopefully) get the following:

```html
<p>Hi, Lyra!</p>
```

Congrats – you've written your first Maud program!
