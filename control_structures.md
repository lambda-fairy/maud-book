# Control structures

Maud provides various control structures for adding dynamic elements to your templates.

## Branching with `$if` and `$else`

```rust
#[derive(PartialEq)]
enum Princess { Celestia, Luna, Cadance, TwilightSparkle }

let user = Princess::Celestia;

html! {
    $if user == Princess::Luna {
        h1 "Super secret woona to-do list"
        ul {
            li "Nuke the Crystal Empire"
            li "Kick a puppy"
            li "Evil laugh"
        }
    } $else if user == Princess::Celestia {
        p "Sister, please stop reading my diary. It's rude."
    } $else {
        p "Nothing to see here; move along."
    }
}
```

Use `$if` and `$else` to branch on a boolean expression. As with Rust, braces are mandatory and the `$else` clause is optional.

## Looping with `$for`

```rust
let names = ["Applejack", "Rarity", "Fluttershy"];
html! {
    p "My favorite ponies are:"
    ol {
        $for name in names.iter() {
            li $name
        }
    }
}
```

Use `$for .. in ..` to loop over the elements of an iterator.
