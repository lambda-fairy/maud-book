# Using Iron with Maud

Maud includes support for the [Iron] web framework. However, to keep dependencies minimal, this integration is disabled by default.

[Iron]: http://ironframework.io

To enable this support, enable the "iron" feature via Cargo:

```toml
# ...
[dependencies]
maud = { version = "0.11", features = ["iron"] }
maud_macros = "0.11"
# ...
```

Note that the feature must be enabled on the `maud` runtime library, *not* `maud_macros`.

With this feature enabled, you can then build a `Response` from a `Markup` object directly. Here's an example application using Iron and Maud:

```rust
#![feature(plugin)]
#![plugin(maud_macros)]

extern crate iron;
extern crate maud;

use iron::prelude::*;
use iron::status;

fn main() {
    Iron::new(|r: &mut Request| {
        let markup = html! {
            h1 "Hello, world!"
            p {
                "You are viewing the page at " (r.url)
            }
        };
        Ok(Response::with((status::Ok, markup)))
    }).http("localhost:3000").unwrap();
}
```
