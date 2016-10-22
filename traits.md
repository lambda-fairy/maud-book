# The `Render` trait

By default, a `(splice)` is rendered using the [`std::fmt::Display`][Display] trait, with any HTML special characters escaped automatically.

To change this behavior, you can implement the [`Render`][Render] trait for your type. Then, when a value of this type is used in a template, Maud will call your custom code instead.

Below are some examples of using `Render`. Feel free to use these snippets in your own project!

## Example: a shorthand for including CSS stylesheets

When writing a web page, it can be annoying to write `link rel="stylesheet"` over and over again. This example provides a shorthand for linking to CSS stylesheets.

```rust
use maud::{Markup, Render};

/// Links to a CSS stylesheet at the given path.
struct Css(&'static str);

impl Render for Css {
    fn render(&self) -> Markup {
        html! {
            link rel="stylesheet" type="text/css" href=(self.0) /
        }
    }
}
```

## Example: a wrapper that calls `std::fmt::Debug`

When debugging an application, it can be useful to see its internal state. But these internal data types often don't implement `Display`. This wrapper lets us use the [`Debug`][Debug] trait instead.

To avoid extra allocation, we override the `.render_to()` method instead of `.render()`. This doesn't do any escaping by default, so we wrap the output in an `Escaper` as well.

```rust
use maud::{Render, Escaper};
use std::fmt;

/// Renders the given value using its `Debug` implementation.
struct Debug<T: fmt::Debug>(T);

impl<T: fmt::Debug> Render for Debug<T> {
    fn render_to(&self, output: &mut String) {
        let mut escaper = Escaper::new(output);
        write!(escaper, "{:?}", self.0).unwrap();
    }
}
```

## `RenderOnce`

The `Render` trait takes its argument by reference (`&self`). But sometimes, we need to take the parameter by value instead.

This use case is supported through the [`RenderOnce`][RenderOnce] trait.

TODO: include an example

[Debug]: https://doc.rust-lang.org/std/fmt/trait.Debug.html
[Display]: https://doc.rust-lang.org/std/fmt/trait.Display.html
[Render]: https://docs.rs/maud/*/maud/trait.Render.html
[RenderOnce]: https://docs.rs/maud/*/maud/trait.RenderOnce.html
