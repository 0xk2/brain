---
{"dg-publish":true,"permalink":"/solana/state-competing/"}
---

Imagine this situation

```
Bob   ---> |
Alice ---> | --> Carol
Manny ---> |
```

If a thousand people are paying `Carol`, then each transaction must be queued, and some transactions will fail to be executed.
Consider the complexity of Carol, a program-owned account for fee collectors or fee miners. In such a scenario, the likelihood of this situation is high. The proposed solution is to divide the instruction into 2 steps.
Step 1: Pay a virtual account which is owned by our program
Step 2: Crank to withdraw money to `Carol`

The most optimal way is for each payer to create virtual accounts to avoid bottlenecks. However, if the payer is also a program that needs to run in parallel, it still makes a bottleneck!
To solve this, we have to look at the context of the program and design in a way that each transaction uses its account!