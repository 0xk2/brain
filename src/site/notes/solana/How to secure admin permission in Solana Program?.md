---
{"dg-publish":true,"permalink":"/solana/how-to-secure-admin-permission-in-solana-program/"}
---

## How `solana deploy` works

`BPFLoaderUpgradeab1e11111111111111111111111` will create & own these accounts (files)

![Screenshot 2024-04-29 at 18.44.18.png](/img/user/images/Screenshot%202024-04-29%20at%2018.44.18.png)

The Program account is the account of which `program.key()` is the address to identify the program, or `programId: PubKey` in ts library. In Program account, it hold `program_data: PubKey`, which is the address of ProgramData account. 
The ProgramData account is the account that holds the Program's executable data.
Buffers are accounts that hold chunks of data to write into ProgramData to by-pass the 1232 limit of each Solana Transaction.

## Secure admin permission in a Solana Program

Each Solana program has an `upgrade_authority_address`, which can change the content of ProgramData. Once the program is stable, we can set the `upgrade_authority_address` to `null` to make it immutable. Do this through `BPFLoaderUpgradeab1e11111111111111111111111` program.

We might want the `admin` is separated from upgrade_authority_address. However, if we want to make admin (or authority in the demo code) IS the `upgrade_authority_address`, we can use build a function to init admin config, with the following context struct:

![Screenshot 2024-04-29 at 18.44.33.png](/img/user/images/Screenshot%202024-04-29%20at%2018.44.33.png)

**Explain:**
`authority` is the signer and admin.
`program` is the Program account.
`program_data` is the ProgramData account.

The constraint 
`constraint = program.programdata_address()? = Some(program_data.key())`  make sure that the program & program_data pair is correct.
The contraint
`constraint = program_data.upgrade_authority_address == Some(authority.key())` make sure that the authority is the `upgrade_authority_address`

## Conclusion

This approach makes it easier to manage admin permission with only one address. However, it ties the development and operation aspects of a product into one account, not necessary something I want to propose to any scalable software.
