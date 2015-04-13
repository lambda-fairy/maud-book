# Basic syntax

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
}
```

Literal strings use the same syntax as Rust. Wrap them in double quotes, and use a backslash for escapes.

```rust
html! {
    "<script>alert(\"XSS\")</script>"  // &lt;script&gt;...
    $$"<script>alert(\"XSS\")</script>"  // <script>...
}
```

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
}
```

Write an element using curly braces: `p {}`.

Terminate a void element using a slash: `br /`.

```rust
html! {
    p small em "squee"
}
```

If the element has only a single child, you can omit the brackets. This shorthand works with nested elements too.

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
    p contenteditable?=allow_editing {
        "Edit me, I " em "dare" " you."
    }
}
```

To toggle an attribute based on a boolean flag, use a `?=` suffix instead: `checked?=foo`. This will check the value of `foo` at runtime, inserting the attribute only if `foo` is `true`.

## Splices

```rust
let best_pony = "Pinkie Pie";
let numbers = [1, 2, 3, 4];
let secret_message = "Surprise!";
let pre_escaped = "<p>Pre-escaped</p>";
html! {
    h1 { $best_pony " says:" }
    p {
        "I have " $numbers.len() " numbers, "
        "and the first one is " $numbers[0]
    }
    p title=$secret_message {
        "1 + 1 = " $(1 + 1)
    }
    $$pre_escaped
}
```

TODO