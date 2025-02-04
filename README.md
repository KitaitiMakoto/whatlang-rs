# Whatlang

[![Build Status](https://travis-ci.org/greyblake/whatlang-rs.svg?branch=master)](https://travis-ci.org/greyblake/whatlang-rs)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/greyblake/whatlang-rs/master/LICENSE)
[![Documentation](https://docs.rs/whatlang/badge.svg)](https://docs.rs/whatlang) [![Demo](https://img.shields.io/badge/online-demo-yellow.svg)](https://www.greyblake.com/whatlang)

Natural language detection for Rust with focus on simplicity and performance.

* [Features](#features)
* [Get started](#get-started)
* [Documentation](https://docs.rs/whatlang)
* [Supported languages](https://github.com/greyblake/whatlang-rs/blob/master/SUPPORTED_LANGUAGES.md)
* [Requirements](#requirements)
* [How does it work?](#how-does-it-work)
  * [How language recognition works?](#how-language-recognition-works)
  * [How is_reliable calculated?](#how-is_reliable-calculated)
* [Running benchmark](#running-benchmarks)
* [Comparison with alternatives](#comparison-with-alternatives)
* [Ports and clones](#ports-and-clones)
* [Derivation](#derivation)
* [License](#license)
* [Contributors](#contributors)


## Features
* Supports [87 languages](https://github.com/greyblake/whatlang-rs/blob/master/SUPPORTED_LANGUAGES.md)
* 100% written in Rust
* Lightweight, fast and simple
* Recognizes not only a language, but also a script (Latin, Cyrillic, etc)
* Provides reliability information

## Get started

Add to you `Cargo.toml`:
```
[dependencies]

whatlang = "0.11.1"
```

Example:

```rust
extern crate whatlang;

use whatlang::{detect, Lang, Script};

fn main() {
    let text = "Ĉu vi ne volas eklerni Esperanton? Bonvolu! Estas unu de la plej bonaj aferoj!";

    let info = detect(text).unwrap();
    assert_eq!(info.lang(), Lang::Epo);
    assert_eq!(info.script(), Script::Latin);
    assert_eq!(info.confidence(), 1.0);
    assert!(info.is_reliable());
}
```

For more details (e.g. how to blacklist some languages) please check the [documentation](https://docs.rs/whatlang).

## Requirements

The latest whatlang library works with rust 1.31.0 or higher.

## How does it work?

### How does the language recognition work?

The algorithm is based on the trigram language models, which is a particular case of n-grams.
To understand the idea, please check the original whitepaper [Cavnar and Trenkle '94: N-Gram-Based Text Categorization'](https://www.researchgate.net/publication/2375544_N-Gram-Based_Text_Categorization).

### How _is_reliable_ calculated?

It is based on the following factors:
* How many unique trigrams are in the given text
* How big is the difference between the first and the second(not returned) detected languages? This metric is called `rate` in the code base.

Therefore, it can be presented as 2d space with threshold functions, that splits it into "Reliable" and "Not reliable" areas.
This function is a hyperbola and it looks like the following one:

<img alt="Language recognition whatlang rust" src="https://raw.githubusercontent.com/greyblake/whatlang-rs/master/misc/images/whatlang_is_reliable.png" width="450" height="300" />

For more details, please check a blog article [Introduction to Rust Whatlang Library and Natural Language Identification Algorithms](https://www.greyblake.com/blog/2017-07-30-introduction-to-rust-whatlang-library-and-natural-language-identification-algorithms/).

## Running benchmarks

This is mostly useful to test performance optimizations.

```
cargo bench
```

## Comparison with alternatives

|                           | Whatlang   | CLD2        | CLD3           |
| ------------------------- | ---------- | ----------- | -------------- |
| Implementation language   | Rust       | C++         | C++            |
| Languages                 | 87         | 83          | 107            |
| Algorithm                 | trigrams   | quadgrams   | neural network |
| Supported Encoding        | UTF-8      | UTF-8       | ?              |
| HTML support              | no         | yes         | ?              |


## Ports and clones

* [whatlang-ffi](https://github.com/greyblake/whatlang-ffi) - C bindings
* [whatlanggo](https://github.com/abadojack/whatlanggo) - whatlang clone for Go language
* [whatlang-py](https://github.com/cathalgarvey/whatlang-py) - bindings for Python

## Derivation

**Whatlang** is a derivative work from [Franc](https://github.com/wooorm/franc) (JavaScript, MIT) by [Titus Wormer](https://github.com/wooorm).

## License

[MIT](https://github.com/greyblake/whatlang-rs/blob/master/LICENSE) © [Sergey Potapov](http://greyblake.com/)


## Contributors

- [greyblake](https://github.com/greyblake) Potapov Sergey - creator, maintainer.
- [Dr-Emann](https://github.com/Dr-Emann) Zachary Dremann - optimization and improvements
- [BaptisteGelez](https://github.com/BaptisteGelez) Baptiste Gelez - improvements
- [goodsauce](goodsauce) - improvements
