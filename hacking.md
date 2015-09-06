# Hacking on Maud

## Debugging output

Maud provides an alternative macro named `html_debug!`. It works exactly as `html!` does, but prints extra diagnostics during compilation.

For example, if we build this code:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate maud;
use maud::Utf8Writer;

use std::io;

fn main() {
    let name = "Lyra";
    let output = Utf8Writer::new(io::stdout());
    html_debug!(output, {
        p { "Hi, " $name "!" }
    }).unwrap();
}
```

We then get:

```
$ cargo build
   Compiling lyra v0.1.0 (file:///home/chris/dev/maud/lyra)
src/main.rs:12:5: 14:7 note: expansion:
{
    let mut __maud_result = Ok(());
    __maud_loop_label:
        loop  {
            use std::fmt::Write;
            match &mut output {
                __maud_writer => {
                    match __maud_writer.write_str("<p>Hi, ") {
                        Ok(()) => { }
                        Err(e) => {
                            __maud_result = Err(e);
                            break __maud_loop_label ;
                        }
                    }
                    match write!(:: maud:: rt:: Escaper {
                                 inner : __maud_writer } , "{}" , name) {
                        Ok(()) => { }
                        Err(e) => {
                            __maud_result = Err(e);
                            break __maud_loop_label ;
                        }
                    }
                    match __maud_writer.write_str("!</p>") {
                        Ok(()) => { }
                        Err(e) => {
                            __maud_result = Err(e);
                            break __maud_loop_label ;
                        }
                    }
                }
            }
            break __maud_loop_label ;
        }
    __maud_result
}
src/main.rs:12     html_debug!(output, {
src/main.rs:13         p { "Hi, " $name "!" }
src/main.rs:14     }).unwrap();
```