# Hacking on Maud

## Debugging output

Maud provides an alternative macro named `html_debug!`. It works exactly as `html!` does, but prints extra diagnostics during compilation.

For example, if we build this code:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate maud;
use std::io;

fn main() {
    let name = "Lyra";
    let markup = html_debug! {
        p { "Hi, " $name "!" }
    };
    markup.render(&mut io::stdout()).unwrap();
}
```

We then get:

```text
$ cargo build

   Compiling lyra v0.0.1 (file:///home/chris/dev/maud/lyra)
src/main.rs:9:18: 11:7 note: expansion:
::maud::rt::make_markup(|w: &mut ::std::fmt::Write| ->
                            Result<(), ::std::fmt::Error> {
                        match w.write_str("<p>Hi, ") {
                            ::std::result::Result::Ok(__try_var) => __try_var,
                            ::std::result::Result::Err(__try_var) =>
                            return ::std::result::Result::Err(__try_var),
                        };
                        match ::maud::rt::write_fmt(&mut ::maud::rt::Escaper{inner:
                                                                                 w,},
                                                    name) {
                            ::std::result::Result::Ok(__try_var) => __try_var,
                            ::std::result::Result::Err(__try_var) =>
                            return ::std::result::Result::Err(__try_var),
                        };
                        match w.write_str("!</p>") {
                            ::std::result::Result::Ok(__try_var) => __try_var,
                            ::std::result::Result::Err(__try_var) =>
                            return ::std::result::Result::Err(__try_var),
                        }; Ok(()) })
src/main.rs:9     let markup = html_debug! {
src/main.rs:10         p { "Hi, " $name "!" }
src/main.rs:11     };
```