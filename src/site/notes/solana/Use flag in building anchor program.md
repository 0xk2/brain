---
{"dg-publish":true,"permalink":"/solana/use-flag-in-building-anchor-program/"}
---


in `Cargo.toml`

```toml
[features]
lang = []
```

Then in code, we can use:

```rust
#[cfg(feature = "lang")]
#[constant]
pub const WELCOME_STRING:&str = "Support multiple language!";

#[cfg(not(feature = "lang"))]
#[constant]
pub const WELCOME_STRING:&str = "Does not support multiple language!";
```

the value of `WELCOME_STRING` will change accordingly to the feature flag.
To use the flag, command is:
`anchor test -- --features "lang"`

Note: if you are building multiple programs, the `lang` feature must existed in all programs.