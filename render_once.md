# Streaming content using `RenderOnce`

By default, Maud takes expressions by reference. In other words, every `(splice)` can be thought of as having a `&` borrow in front of it.

While this is what you want most of the time, some applications need to *move* the value instead. One example is a stream of rows from a database, where processing the stream would consume it.

This use case is supported through [`RenderOnce`][RenderOnce]:

TODO: add an example

[RenderOnce]: https://lambda.xyz/maud/doc/maud/trait.RenderOnce.html
