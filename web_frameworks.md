# Web framework integration

Maud includes support for these web frameworks: [Iron], [Rocket], and [Rouille].

[Iron]: http://ironframework.io
[Rocket]: https://rocket.rs/
[Rouille]: https://github.com/tomaka/rouille

# Iron

To keep dependencies minimal, Iron support is disabled by default. To enable it, use the "iron" feature via Cargo:

```toml
# ...
[dependencies]
maud = { version = "*", features = ["iron"] }
# ...
```

With this feature enabled, you can then build a `Response` from a `Markup` object directly. Here's an example application using Iron and Maud:

```rust
#![feature(proc_macro)]

extern crate iron;
extern crate maud;

use iron::prelude::*;
use iron::status;
use maud::html;

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

`Markup` will set the content type of the response automatically, so you don't need to add it yourself.

# Rocket

Rocket works in a similar way, except using the `rocket` feature:

```toml
# ...
[dependencies]
maud = { version = "*", features = ["rocket"] }
# ...
```

This adds a `Responder` implementation for the `Markup` type, so you can return the result directly:

```rust
#![feature(plugin)]
#![feature(proc_macro)]
#![plugin(rocket_codegen)]

extern crate maud;
extern crate rocket;

use maud::{html, Markup};

#[get("/<name>")]
fn hello(name: &str) -> Markup {
    html! {
        h1 { "Hello, " (name) "!" }
        p "Nice to meet you!"
    }
}

fn main() {
    rocket::ignite().mount("/", routes![hello]).launch();
}
```

# Rouille

Unlike with the other frameworks, Rouille doesn't need any extra features at all! Calling `Response::html` on the rendered `Markup` will Just Work®.

```rust
#![feature(proc_macro)]

extern crate maud;
#[macro_use] extern crate rouille;

use maud::html;
use rouille::Response;

fn main() {
    rouille::start_server("localhost:8000", move |request| {
        router!(request,
            (GET) (/{name: String}) => {
                html! {
                    h1 { "Hello, " (name) "!" }
                    p "Nice to meet you!"
                }
            },
            _ => Response::empty_404()
        )
    });
}
```
