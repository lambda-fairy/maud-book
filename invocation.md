# `html!` and `html_utf8!`

Maud provides two macros: `html!` and `html_utf8!`. Both of them take a writer as their first argument, and a template (using Maud's custom syntax) as their second. Where they differ is in the kind of writer they expect:

* As its name implies, `html_utf8!(w, ...)` encodes the output to UTF-8. The writer `w` must accept binary data through the [`std::io::Write`][1] trait. Most I/O libraries build on this trait, so you should be using `html_utf8!` most of the time.

* `html!(w, ...)` does not do any encoding. The writer `w` must accept Unicode text directly, through the [`std::fmt::Write`][2] trait.

  As an example, take the standard `String` type. Since a `String` cannot contain arbitrary binary data, it cannot implement `std::io::Write`. It does implement `std::fmt::Write` though, and it is this feature that lets us append markup to a `String` buffer. In fact, most of Maud's own unit tests are written this way:

  ```rust
  #[test]
  fn element_syntax() {
      let mut buffer = String::new();
      html!(buffer, p "Hello, world!").unwrap();
      assert_eq!(buffer, "<p>Hello, world!</p>");
  }
  ```

[1]: http://doc.rust-lang.org/std/io/trait.Write.html
[2]: http://doc.rust-lang.org/std/fmt/trait.Write.html
