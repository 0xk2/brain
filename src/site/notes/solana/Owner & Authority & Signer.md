---
{"dg-publish":true,"permalink":"/solana/owner-and-authority-and-signer/"}
---


Each account has the following fields:
- Lamport: how many Lamport does the account hold?
- Owner: a public key; which can be a program or a private-key pair. If the owner of an account is a public from a private-key then you cannot modify the content (Lamport, Owner, Data) of this account.
- Data: only owner can modify the data
If an account has Data that is not 0 then you cannot change the owner to something else!

This mechanism ensure that once you do an owner check, you can trust the data inside that account!

Signer is the one "sign" the transaction to prove that he own the public key.

Three important check:
- Owner check ~> ran by runtime environment
- Signer check -> ran by runtime environment
- Authority check -> custom data field to make sure whoever write data has sufficient permission