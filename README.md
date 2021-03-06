Serde Pickle Serialization Library
==================================

[![Build status](https://api.travis-ci.org/birkenfeld/serde-pickle.png)](https://travis-ci.org/birkenfeld/serde-pickle)
[![Latest Version](https://img.shields.io/crates/v/serde-pickle.svg)](https://crates.io/crates/serde-pickle)

[Documentation](https://birkenfeld.github.io/serde-pickle/serde_pickle/index.html)

This crate is a Rust library for parsing and generating Python pickle
streams. It is built upon [Serde](https://github.com/serde-rs/serde), a high
performance generic serialization framework.

Installation
============

This crate works with Cargo and can be found on
[crates.io](https://crates.io/crates/serde-pickle) with a `Cargo.toml` like:

```toml
[dependencies]
serde = "*"
serde-pickle = "*"
```

Usage
=====

As with other serde serialization implementations, this library provides
toplevel functions for simple en/decoding of supported objects.

Example:

```rust
extern crate serde;
extern crate serde_pickle;

use std::collections::BTreeMap;

fn main() {
    let mut map = BTreeMap::new();
    map.insert("x".to_string(), 1.0);
    map.insert("y".to_string(), 2.0);

    // Serialize the map into a pickle stream.
    // The second argument selects pickle version 3.
    let serialized = serde_pickle::to_vec(&map, true).unwrap();

    // Deserialize the pickle stream back into a map.
    // Because we compare it to the original `map` below, Rust infers
    // the type of `deserialized` and lets serde work its magic.
    let deserialized = serde_pickle::from_slice(&serialized).unwrap();
    assert_eq!(map, deserialized);
}
```

Serializing and deserializing structs and enums that implement the
serde-provided traits is supported, and works analogous to other crates
(using either `serde_macros` or `serde_codegen`).

Notes
=====

The test suite currently only works with the nightly-based `serde_macros`.
