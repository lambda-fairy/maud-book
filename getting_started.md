# Getting started

First off, add `maud` to your `Cargo.toml`:

```toml
[dependencies]
# ...
maud = "*"
```

Now save the following to `src/main.rs`:

```rust
#![feature(proc_macro_non_items)]
#![feature(use_extern_macros)]  // <- IMPORTANT don't forget this!

extern crate maud;
use maud::html;

fn main() {
    let name = "Lyra";
    let markup = html! {
        p { "Hi, " (name) "!" }
    };
    println!("{}", markup.into_string());
}
```

`html!` takes a single argument: a template using Maud's custom syntax. This call expands to an expression of type [`Markup`][Markup], which can then be converted to a `String` using `.into_string()`.

[Markup]: https://docs.rs/maud/*/maud/type.Markup.html

Run this program with `cargo run`, and you'll (hopefully) get the following:

```
<p>Hi, Lyra!</p>
```

Congrats – you've written your first Maud program!
