# Basic syntax

The next few sections will outline the syntax used by Maud templates.

## Literals `""`

```rust
html! {
    "Oatmeal, are you crazy?"
}
```

Literal strings use the same syntax as Rust. Wrap them in double quotes, and use a backslash for escapes.

```rust
use maud::PreEscaped;
html! {
    "<script>alert(\"XSS\")</script>"                // &lt;script&gt;...
    (PreEscaped("<script>alert(\"XSS\")</script>"))  // <script>...
}
```

By default, HTML special characters are escaped automatically. Wrap the string in `(PreEscaped())` to disable this escaping. (This is a special case of the *splice* syntax described below.)

## Elements `p`

```rust
html! {
    h1 "Poem"
    p {
        "Rock, you are a rock."
        br /
        "Gray, you are gray,"
        br /
        "Like a rock, which you are."
        br /
        "Rock."
    }
}
```

Write an element using curly braces: `p { ... }`.

Terminate a void element using a slash: `br /`. Note that the result will be rendered with HTML syntax – `<br>` not `<br />`.

```rust
html! {
    p small em "squee"
}
```

If the element has only a single child, you can omit the brackets. This shorthand works with nested elements too.

## Splices `(foo)`

```rust
let best_pony = "Pinkie Pie";
let numbers = [1, 2, 3, 4];
html! {
    p { "Hi, " (best_pony) "!" }
    p {
        "I have " (numbers.len()) " numbers, "
        "and the first one is " (numbers[0])
    }
}
```

Use `(foo)` syntax to splice in the value of `foo` at runtime. Any HTML special characters are escaped by default.

You can splice any value that implements [`std::fmt::Display`][Display]. Most primitive types (such as `str` and `i32`) implement this trait, so they should work out of the box. To change this behavior for some type, you can implement the [`Render`][Render] trait by hand. See the [traits](./traits.md) section for details.

[Display]: http://doc.rust-lang.org/std/fmt/trait.Display.html
[Render]: https://docs.rs/maud/*/maud/trait.Render.html

```rust
use maud::PreEscaped;
let post = "<p>Pre-escaped</p>";
html! {
    h1 "My super duper blog post"
    (PreEscaped(post))
}
```

To disable automatic escaping, use the [`PreEscaped`][PreEscaped] wrapper.

[PreEscaped]: https://lambda.xyz/maud/maud/struct.PreEscaped.html

## Non-empty attributes `id="yay"`

```rust
html! {
    ul {
        li a href="about:blank" "Apple Bloom"
        li class="lower-middle" "Sweetie Belle"
        li dir="rtl" { "Scootaloo " small "(also a chicken)" }
    }
}
```

Add attributes using the syntax: `attr="value"`. You can attach any number of attributes to an element. The values must be quoted: they are parsed as string literals.

## Splices in attributes `title=(secret_message)`

```rust
let secret_message = "Surprise!";
html! {
    p title=(secret_message) {
        "Nothing to see here, move along."
    }
}
```

Splices work in attributes as well.

```rust
const GITHUB: &'static str = "https://github.com";
html! {
    a href={ (GITHUB) "/lfairy/maud" } {
        "Fork me on GitHub"
    }
}
```

To concatenate multiple values within an attribute, wrap the whole thing in braces. This syntax is useful for building URLs.

## Empty attributes `checked?` `disabled?[foo]`

```rust
html! {
    form {
        input type="checkbox" name="cupcakes" checked? /
        " "
        label for="cupcakes" "Do you like cupcakes?"
    }
}
```

Declare an empty attribute using a `?` suffix: `checked?`.

```rust
let allow_editing = true;
html! {
    p contenteditable?[allow_editing] {
        "Edit me, I " em "dare" " you."
    }
}
```

To toggle an attribute based on a boolean flag, use a `?[]` suffix instead: `checked?[foo]`. This will check the value of `foo` at runtime, inserting the attribute only if `foo` is `true`.

## Classes and IDs `.foo` `#bar`

```rust
html! {
    div.container#main {
        input.big.scary.bright-red type="button" value="Launch Party Cannon" /
    }
}
```

Add classes and IDs to an element using `.foo` and `#bar` syntax. You can chain multiple classes and IDs together, and mix and match them with other attributes.

## Comments `//` `/* */`

```rust
html! {
    // This text is ignored
    p "Hello!"
    /* This as well */
}
```

Rust comments work as expected.
