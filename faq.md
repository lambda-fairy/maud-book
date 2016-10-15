# Frequently asked questions

## What is the origin of the name "Maud"?

Maud is named after a [character](http://mlp.wikia.com/wiki/Maud_Pie) from *My Little Pony: Friendship is Magic*. It does not refer to the [poem](https://en.wikipedia.org/wiki/Maud_and_other_poems) by Alfred Tennyson, though other people have brought that up in the past.

Here are some reasons why I chose this name:

* "Maud" shares three letters with "markup";

* The library is efficient and austere, like the character;

* There is a notable website called ["HTML5 Rocks"](http://www.html5rocks.com), and Maud (the character) is a geologist.

## Why does `html!` always allocate a `String`? Wouldn't it be more efficient if it wrote to a handle directly?

Good question! In fact, Maud did work this way in the past; but in the end I found it was better to drop this feature.

First, having to deal with an extra "writer" argument added to the cognitive overhead of the library, more so when features like sub-templates were involved. Furthermore, since frameworks like Iron need to take ownership of the response body, we often ended up having to allocate anyway. To put the nail in the coffin, benchmarks showed that a `String` based solution is actually faster than one which avoids allocations.

For these reasons, I changed `html!` to return a `String` in version 0.11.

## Why is Maud written as a compiler plugin? Can't it use `macro_rules!` instead?

This is certainly possible, and in fact the [Horrorshow](https://github.com/Stebalien/horrorshow-rs) library works this way.

I use compiler plugins because they are more flexible. There are some syntax constructs in Maud that cannot be parsed with `macro_rules!`; better diagnostics are a bonus as well.
