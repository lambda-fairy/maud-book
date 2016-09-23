# Maud: a compile-time template engine for Rust

```rust
html! {
    h1 "Hello, world!"
    p.intro {
        "This is an example of the "
        a href="https://github.com/lfairy/maud" "Maud"
        " template language."
    }
}
```

Maud is an HTML template engine for Rust. It's implemented as a macro, `html!`, which compiles your markup to specialized Rust code. This unique approach makes Maud templates blazing fast, super type-safe, and easy to deploy.

## Why use Maud?

### Tight integration with Rust

Since Maud is a Rust macro, it can borrow most of its features from the host language. Pattern matching and `for` loops work as they do in Rust. There is no need to derive JSON conversions, as your templates can work with Rust values directly.

### Type safety

Your templates are checked by the compiler, just like the code around them. Any typos will be caught at compile time, not after your app has already started.

### Minimal runtime

Since most of the work happens at compile time, the runtime footprint is small. The Maud runtime library, including integration with the [Iron] web framework, is under 100 SLoC.

[Iron]: http://ironframework.io/

### Simple deployment

There is no need to track separate template files, since all relevant code is linked into the final executable.

## Why *not* use Maud?

### Requires Nightly compiler

Maud depends on the unstable [syntax extension API], and so you'll need to install the Nightly version of Rust to use it.

There is a new—stable—macro API [in the works][macros 2.0], but it'll be a while before that's ready.

[syntax extension API]: https://doc.rust-lang.org/book/compiler-plugins.html
[macros 2.0]: http://www.ncameron.org/blog/macros-in-rust-pt5/

### Tight integration with Rust

Yes, it's a drawback too! The Rust-like syntax can be off-putting to designers used to working with plain HTML. Also, it can be inconvenient to iterate on a design, since every edit would trigger a recompile of the containing crate.
