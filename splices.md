# Dynamic content

## Splice syntax `^foo` `^(foo)`

```rust
let best_pony = "Pinkie Pie";
html! {
    p { "Hi, " ^best_pony "!" }
}
```

Use `^foo` syntax to splice in the value of `foo` at runtime.

You can splice any value that implements [`std::fmt::Display`][Display]. Most primitive types (such as `str` and `i32`) implement this trait, so they should work out of the box.

[Display]: http://doc.rust-lang.org/std/fmt/trait.Display.html

```rust
let numbers = [1, 2, 3, 4];
html! {
    p {
        "I have " ^numbers.len() " numbers, "
        "and the first one is " ^numbers[0]
    }
}
```

Indexing (`^foo[0]`), method calls (`^foo.method(arg)`), and property lookups (`^foo.property`) work as you'd expect.

```rust
let secret_message = "Surprise!";
html! {
    p title=^secret_message {
        "Nothing to see here, move along."
    }
}
```

Splices work in attributes as well.

```rust
const GITHUB: &'static str = "https://github.com";
html! {
    a href={ ^GITHUB "/lfairy/maud" } {
        "Fork me on GitHub"
    }
}
```

To concatenate multiple values within an attribute, wrap the whole thing in braces. This syntax is useful for building URLs.

```rust
html! {
    p {
        "The answer is " ^(27 + 15) "."
    }
}
```

You can splice more complex expressions by wrapping them in parentheses: `^(foo)`.

## Automatic escaping

```rust
use maud::PreEscaped;
let post = "<p>Pre-escaped</p>";
html! {
    h1 "My super duper blog post"
    ^PreEscaped(post)
}
```

Maud escapes HTML special characters by default. To disable this escaping, use the [`PreEscaped`][PreEscaped] wrapper.

[PreEscaped]: https://lambda.xyz/maud/doc/maud/struct.PreEscaped.html

## Streaming content

By default, Maud takes expressions by reference. In other words, every `^` splice can be thought of as having a `&` borrow in front of it.

While this is what you want most of the time, some applications need to *move* the value instead. One example is a stream of rows from a database, where processing the stream would consume it.

This use case is supported through [`RenderOnce`][RenderOnce]:

TODO: add an example

[RenderOnce]: https://lambda.xyz/maud/doc/maud/trait.RenderOnce.html
