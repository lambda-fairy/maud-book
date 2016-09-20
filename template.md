# Subtemplates using `template!`

As your project grows larger, it can become useful to factor out common parts of your templates. In general, Maud tries to stay as close as possible to the host language, and this use case is no exception. Where other systems use "inheritance" or "partials", Maud just uses plain Rust closures.

TODO explain the [Template] trait, along with everything else

[Template]: https://lambda.xyz/maud/maud/trait.Template.html
