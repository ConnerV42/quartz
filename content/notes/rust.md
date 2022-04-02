---
title: "Rust"
tags:
- software-engineering
- rust
disableToc: false
---

## Rust

Without the need to have a garbage collector continuously running, Rust projects are well-suited to be used as libraries by other programming languages via foreign-function interfaces. This allows existing projects to replace performance-critical pieces with speedy Rust code without the memory safety risks inherent with other systems programming languages. Some projects have even been [incrementally rewritten in Rust](https://people.gnome.org/~federico/blog/librsvg-is-almost-rustified.html) using these techniques.

Systems programming languages have the implicit expectation that they will be around effectively forever. While some modern development doesn’t require that amount of longevity, many businesses want to know that their fundamental code base will be usable for the foreseeable future. Rust recognizes this and has made conscious design decisions around backwards compatibility and stability; it’s a language [designed for the next 40 years](https://www.youtube.com/watch?v=A3AdN7U24iU).

Major highlights of web development with Rust are:
- You can compile Rust to [WebAssembly](https://simpleprogrammer.com/webassembly-finally-freed-javascript/) so it's easier to get near-native performance on the web.
- Rust allows any language to compile into WebAssembly, thus allowing for portable, executable running code online.