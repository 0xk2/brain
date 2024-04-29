---
{"dg-publish":true,"permalink":"/solana/loading-big-data-in-solana/"}
---


### Account vs AccountLoader
Account loads all data from memory to the Stack
AccountLoader does not load any data; it will lazily load later

### ZeroCopy

Watch this for detail: https://www.youtube.com/watch?v=zs_yU0IuJxc
or https://youtu.be/OOITlLRG8H4?si=ihut_ODywXvSN90I&t=2502

```
#[account(zero_copy(unsafe))]

pub struct SomeBigDataStruct {

	pub big_data: [u8; 5000],
}

  

#[derive(Accounts)]

pub struct SomeFunctionContext<'info> {

	#[account(zero)]
	
	pub some_big_data: AccountLoader<'info, SomeBigDataStruct>,

}
```

To init, use `load_init`, then you can write. After the data is inited, use `load_mut` to write.

### Struct size for `future use`

If you create a program and want to update it in the future and you want account design is future proof. There are two tactics.

Use `padding` for future usage if you want dynamic field in your struct:

```
// First version
pub struct GameState {
	pub health: u64,
	pub mana: u64,
	pub for_future_usage: [u8, 128],
	pub event_log: Vec<String>
}
```

```
// Second version
pub struct GameState {
	pub health: u64,
	pub mana: u64,
	pub experience: u64,
	pub for_future_usage: [u8, 120], // 128 - (64/8)
	pub event_log: Vec<String>
}
```

Or you can set all items in your struct fixed-length:

```
// First version
pub struct GameState {
	pub health: u64,
	pub mana: u64,
}

// Second version
pub struct GameState {
	pub health: u64,
	pub mana: u64,
	pub new_property: u8
}
```
==Code to check if this is possible!==
# Question

1. What do `InitSpace` & `max_len` do?
   Look at this: https://docs.rs/anchor-derive-space/0.30.0/src/anchor_derive_space/lib.rs.html#39. Need an experiment to see the size of the Account.
   With `derive_init_space`, it returns `TokenStream(expanded)`, where `expanded` is the number of bytes. This function seems to collect and rent out memory immediately.
2. Is it possible to create a fixed-length string in Solana?
   Possibly, use a [u8, 100] to store a 100 characters string, each character occupying 1 byte
