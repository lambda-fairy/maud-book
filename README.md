Meet Maud
=========

Maud is an experimental template language for Rust. It's implemented as a macro, `html!`, which compiles your markup to specialized Rust code.

You've probably used the `println!` or `regex!` macros before. Maud takes the same idea, and applies it to HTML templates. If you make a typo in your markup, the compiler will tell you straight away -- instead of your app crashing at runtime.

Another advantage is performance. Since everything is decided at compile time, there are no hash maps, no intermediate structures, no runtime checking to slow the code down. In fact, Maud can run just fine without a heap, as all its runtime structures are unboxed.

There is one drawback though: Maud relies on very unstable compiler APIs, which are unlikely to make it into Rust 1.0. You'll need to install the Nightly version of Rust instead.

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