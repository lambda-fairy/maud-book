# Maud: a compile-time template engine for Rust

```rust
html! {
    h1 { "Hello, world!" }
    p.intro {
        "This is an example of the "
        a href="https://github.com/lfairy/maud" { "Maud" }
        " template language."
    }
}
```

Maud is an HTML [template engine] for Rust. It's implemented as a macro, `html!`, which compiles your markup to specialized Rust code. This unique approach makes Maud templates blazing fast, super type-safe, and easy to deploy.

[template engine]: https://www.simple-is-better.org/template/

## Why use Maud?

### Tight integration with Rust

Since Maud is a Rust macro, it can borrow most of its features from the host language. Pattern matching and `for` loops work as they do in Rust. There is no need to derive JSON conversions, as your templates can work with Rust values directly.

### Type safety

Your templates are checked by the compiler, just like the code around them. Any typos will be caught at compile time, not after your app has already started.

### Minimal runtime

Since most of the work happens at compile time, the runtime footprint is small. The Maud runtime library, including integration with the [Iron] and [Rocket] web frameworks, is around 100 SLoC.

[Iron]: http://ironframework.io/
[Rocket]: https://rocket.rs/

### Simple deployment

There is no need to track separate template files, since all relevant code is linked into the final executable.

## Why *not* use Maud?

### Requires Nightly compiler

Maud depends on the unstable [procedural macro API], and so you'll need to install the nightly version of Rust to use it.

[procedural macro API]: https://github.com/rust-lang/rust/issues/38356

### Longer compile times

It can be inconvenient to iterate on a design, since every edit would trigger a recompile of the containing crate. When [incremental compilation] lands, though, the difference may not be as noticeable as you'd think.

[incremental compilation]: https://blog.rust-lang.org/2016/09/08/incremental.html
