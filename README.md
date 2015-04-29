# Meet Maud

Maud is an experimental template language for Rust. It's implemented as a macro, `html!`, which compiles your markup to specialized Rust code.

If you've used the `println!` or `regex!` macros before, this concept should sound familiar. Maud takes this same idea, and applies it to HTML templates. If you make a typo in your template, the compiler will tell you straight away – instead of your app crashing at runtime.

Another advantage is performance. Since everything is decided at compile time, there are no hash maps, no intermediate structures, no runtime checking to slow the code down. In fact, Maud can run just fine without a heap, as all its runtime structures are unboxed.

There is one drawback though: Maud depends on the [syntax extension API][1], which is unlikely to make it into Rust 1.0. You'll need to install the Nightly version of Rust to use it.

[1]: https://doc.rust-lang.org/book/compiler-plugins.html