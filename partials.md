# Partials

Maud does not have a built-in concept of partials or sub templates. Instead, you can compose your markup with any function that returns `Markup`.

The following uses a `header` and `footer` function that are used in the `page` function to return a final result.

Here is a page that has a dynamic page title, a body and a footer with an RSS link:

```rust
extern crate maud;

use self::maud::{DOCTYPE, html, Markup};

/// A basic header with a dynamic `page_title`.
fn header(page_title: &str) -> Markup {
    html! {
        (DOCTYPE)
        html {
            meta charset="utf-8";
            title { (page_title) }
        }
    }
}

/// A static footer.
fn footer() -> Markup {
    html! {
        footer {
            span {
                a href="rss.atom" { "RSS Feed" }
            }
        }
    }
}

/// The final Markup, including `header` and `footer`.
pub fn page(title: &str) -> Markup {
    html! {
        // Add the header markup to the page
        (header(title))
        body {
            h1 { "Hello World" }
        }
        // Add the footer markup to the page
        (footer())
    }
}
```

Calling `page` with a title will then return the markup for the whole page.
