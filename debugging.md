# Debugging

Maud provides alternative macros, named `html_debug!` and `html_utf8_debug!`. These work as their debug-less counterparts do, but print extra diagnostics during compilation.

For example, if we build this code:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate maud;

use std::io;

fn main() {
    let name = "Lyra";
    html_utf8_debug!(io::stdout(), {
        p { "Hi, " (name) "!" }
    }).unwrap();
}
```

We then get:

```
$ cargo build
   Compiling lyra v0.1.0 (file:///home/chris/dev/maud/lyra)
src/main.rs:10:5: 12:7 note: expansion:
match ::maud::Utf8Writer::new(&mut io::stdout()) {
    mut __maud_utf8_writer => {
        let _ =
            {
                let mut __maud_result = Ok(());
                __maud_loop_label:
                    loop  {
                        use std::fmt::Write;
                        match &mut __maud_utf8_writer {
                            __maud_writer => {
                                __maud_writer as &mut ::std::fmt::Write;
                                match __maud_writer.write_str("<p>Hi, ") {
                                    Ok(()) => { }
                                    Err(e) => {
                                        __maud_result = Err(e);
                                        break __maud_loop_label ;
                                    }
                                }
                                match write!(:: maud:: Escaper:: new (
                                             & mut * __maud_writer ) , "{}" ,
                                             name) {
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
            };
        __maud_utf8_writer.into_result()
    }
}
src/main.rs:10     html_utf8_debug!(io::stdout(), {
src/main.rs:11         p { "Hi, " (name) "!" }
src/main.rs:12     }).unwrap();
```
