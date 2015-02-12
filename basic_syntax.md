# Basic syntax

Maud has its own custom syntax, which can be described as the bastard child of Rust and HTML.

## Comments `//` `/* */`

```rust
html! {
    // This text is ignored
    /* This as well */
}
```

Rust comments work as expected. In fact they're stripped by the compiler at the very earliest stage of parsing, before Maud has a chance to see them.

## Literals `""`

```rust
html! {
    "Oatmeal, are you crazy?"
    "<script>alert(\"XSS\")</script>"  // &lt;script&gt;...
    $$"<script>alert(\"XSS\")</script>"  // <script>...
}
```

Literal strings use the same syntax as Rust. Wrap them in double quotes, and use a backslash for escapes.

By default, HTML special characters are escaped automatically. Add a `$$` prefix to disable this escaping. (This is a special case of the *splice* syntax described below.)

## Elements `p {}`

```rust
html! {
    h1 "Pinkie's Brew"
    p {
        "Watch as I work my gypsy magic"
        br /
        "Eye of a newt and cinnamon"
    }
    p small em "squee"
}
```

Write an element using curly braces: `p {}`.

Terminate a void element using a slash: `br /`.

If the element has only a single child, you can omit the brackets: `h1 "Pinkie's Brew"`. This shorthand works with nested elements too.

## Non-empty attributes `id="yay"`

```rust
html! {
    ul id="crusaders" class="truants" {
        li a href="about:blank" "Apple Bloom"
        li class="lower-middle" "Sweetie Belle"
        li dir="rtl" { "Scootaloo " small "(also a chicken)" }
    }
}
```

Add attributes using the syntax: `attr="value"`. You can attach any number of attributes to an element. The values must be quoted: they are parsed as string literals.

## Empty attributes `checked?` `disabled?=foo`

```rust
let allow_editing = true;
html! {
    form {
        input type="checkbox" name="cupcakes" checked? /
        " "
        label for="cupcakes" "Do you like cupcakes?"
    }
    p contenteditable?=allow_editing {
        "Edit me, I " em "dare" " you."
    }
}
```

Declare an empty attribute using a `?` suffix: `checked?`.

To toggle the attribute based on a boolean flag, use a `?=` suffix instead: `checked?=foo`. This will check the value of `foo` at runtime, inserting the attribute only if `foo` is `true`.
