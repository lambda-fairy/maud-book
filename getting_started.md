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
    html_utf8!(io::stdout(), {
        p { "Hi, " $name "!" }
    }).unwrap();
}
```

`html_utf8!` takes two arguments. The first argument is a writer, in this case `io::stdout()`. The second argument is a template using Maud's custom syntax.

Run this program with `cargo run`, and you'll (hopefully) get the following:

```
<p>Hi, Lyra!</p>
```

Congrats – you've written your first Maud program!


## `html!` and `html_utf8!`

Maud provides two macros: `html!` and `html_utf8!`. They use the same syntax, but differ in the types they use.

* As its name implies, `html_utf8!(w, ...)` encodes the output to UTF-8. The writer `w` must accept binary data, through the [`std::io::Write`][1] trait.

* `html!(w, ...)` does not do any encoding. The writer `w` must accept Unicode text directly, through the [`std::fmt::Write`][2] trait.

[1]: http://doc.rust-lang.org/std/io/trait.Write.html
[2]: http://doc.rust-lang.org/std/fmt/trait.Write.html

Most I/O libraries build on `std::io::Write`, and so you should be using `html_utf8!` most of the time.

`html!` is useful when saving the output in a `String`. For example, the code above can be rewritten like this:

```rust
fn main() {
    let name = "Lyra";
    let mut buffer = String::new();
    html!(buffer, {
        p { "Hi, " $name "!" }
    }).unwrap();
    println!("{}", buffer);
}
```
