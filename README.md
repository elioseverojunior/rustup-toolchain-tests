<!--
SPDX-FileCopyrightText: Elio Severo Junior <elioseverojunior@gmail.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# rustup-toolchain-tests

[![crates.io](https://img.shields.io/crates/v/rustup-toolchain-tests.svg)](https://crates.io/crates/rustup-toolchain-tests)
[![docs.rs](https://docs.rs/rustup-toolchain-tests/badge.svg)](https://docs.rs/rustup-toolchain-tests)
[![license: MIT OR Apache-2.0](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0-blue.svg)](#licence)

Generate `.gitignore` files from a declarative TOML specification.

## Status

**This crate is a name reservation.** Version `0.0.0` claims the name on
crates.io; it contains no working API and nothing useful to depend on yet.

Please do not add it as a dependency. The version will move off `0.0.0` when
there is something real to ship, and this README will say so.

## Why

A `.gitignore` is an append-only pile of patterns. Entries accumulate, nobody
remembers which tool needed `**/.cargo-vet` or whether `/target` is still load
bearing, and the same block gets copy-pasted between repositories where it
slowly drifts out of sync.

`rustup-toolchain-tests` treats the file as generated output. Patterns live in TOML with the
reasoning attached, grouped into named sections, and the `.gitignore` is
rendered from that — so the source of truth is reviewable and the output is
reproducible.

## How it works

Describe the sections and rules in `rustup-toolchain-tests.toml`:

```toml
version = 1

[[gitignore.section]]
level = 3
name = "Rust"
note = "Cargo Ignores"

[[gitignore.section.rule]]
ignore = ["/target"]

[[gitignore.section.rule]]
note = "Cargo Package"
ignore = ["**/dist"]

[[gitignore.section.rule]]
note = "Cargo Vet"
ignore = ["**/.cargo-vet"]
```

Which renders to `.gitignore`:

```gitignore
### Rust
# Cargo Ignores
/target

# Cargo Package
**/dist

# Cargo Vet
**/.cargo-vet
```

A section's `name` becomes a heading at the given `level`, each `note` becomes
the comment introducing its rule, and `ignore` carries the patterns themselves.
Every entry keeps the note explaining why it is there.

## Licence

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or
  <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT License ([LICENSE-MIT](LICENSE-MIT) or
  <https://opensource.org/licenses/MIT>)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 licence, shall be
dual licensed as above, without any additional terms or conditions.
