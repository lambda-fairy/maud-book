# Control structures

Maud provides various control structures for adding dynamic elements to your templates.

## Branching with `@if` and `@else`

```rust
#[derive(PartialEq)]
enum Princess { Celestia, Luna, Cadance, TwilightSparkle }

let user = Princess::Celestia;

html! {
    @if user == Princess::Luna {
        h1 "Super secret woona to-do list"
        ul {
            li "Nuke the Crystal Empire"
            li "Kick a puppy"
            li "Evil laugh"
        }
    } @else if user == Princess::Celestia {
        p "Sister, please stop reading my diary. It's rude."
    } @else {
        p "Nothing to see here; move along."
    }
}
```

Use `@if` and `@else` to branch on a boolean expression. As with Rust, braces are mandatory and the `@else` clause is optional.

`@if let` is supported as well. It works as you'd expect:

```rust
let user = Some("Pinkie Pie");
html! {
    p {
        "Hello, "
        @if let Some(name) = user {
            ^name
        } @else {
            "stranger"
        }
        "!"
    }
}
```

## Looping with `@for`

```rust
let names = ["Applejack", "Rarity", "Fluttershy"];
html! {
    p "My favorite ponies are:"
    ol {
        @for name in &names {
            li ^name
        }
    }
}
```

Use `@for .. in ..` to loop over the elements of an iterator.

## Matching with `@match`

```rust
enum Princess { Celestia, Luna, Cadance, TwilightSparkle }

let user = Princess::Celestia;

html! {
    @match user {
        Princess::Luna => {
            h1 "Super secret woona to-do list"
            ul {
                li "Nuke the Crystal Empire"
                li "Kick a puppy"
                li "Evil laugh"
            }
        },
        Princess::Celestia => {
            p "Sister, please stop reading my diary. It's rude."
        },
        _ => {
            p "Nothing to see here; move along."
        }
    }
}
```

Pattern matching is supported with `@match`.

Note that the body of each branch must be delimited by braces `{}`. This means that the following won't work:

```rust
// This won't work!
@match user {
    // ...
    Princess::Celestia => p "..."
    // ...
}
```