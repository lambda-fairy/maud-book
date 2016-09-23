# Debugging

For debugging purposes, Maud provides an alternative macro named `html_debug!`. This macro expands to the same expression as `html!`, but prints extra diagnostics during compilation.

For example, if we build this code:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate maud;

use std::io;

fn main() {
    let name = "Lyra";
    let markup = html_debug! {
        p { "Hi, " (name) "!" }
    };
    println!("{}", markup);
}
```

We then get:

```
$ cargo build
   Compiling lyra v0.1.0 (file:///home/chris/lyra)
warning: expansion:
{
    let mut __maud_writer = ::std::string::String::new();
    let _ = __maud_writer.push_str("<p>Hi, ");
    let _ =
        {
            use ::maud::RenderOnce;
            name.render_once(&mut __maud_writer)
        };
    let _ = __maud_writer.push_str("!</p>");
    ::maud::PreEscaped(__maud_writer)
}
  --> src/main.rs:8:18
   |
8  |     let markup = html_debug! {
   |                  ^

    Finished debug [unoptimized + debuginfo] target(s) in 3.95 secs
```
