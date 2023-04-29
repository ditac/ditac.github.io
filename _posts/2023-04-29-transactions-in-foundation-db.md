---
title: Transactions in FoundatidDB
tags: [transaction, fdb]
style: fill
color: warning
description: Transactions in FoundationDB
---

Lets talk about how sequencers and resolvers work together in FoundationDB to ensure the consistency of transactions.

Sequencers are like bouncers at a club. They're responsible for assigning each transaction a unique, strictly increasing ID to make sure that nobody gets in without an invitation. When you initiate a new transaction, you request an ID from the sequencer, and it gives you one that hasn't been used yet. This way, there's no confusion or overlap between different transactions.

But just like at a club, conflicts can arise even with the best planning. That's where resolvers come in. They're like the peacekeepers of FoundationDB, responsible for resolving conflicts between transactions. If two transactions try to make conflicting changes to the same piece of data, the resolvers step in to make sure that the situation gets resolved fairly.

And here's the kicker: you can customize the conflict resolution logic to suit your needs. Want to settle conflicts with a dance-off? Or maybe with a game of rock-paper-scissors? The choice is yours! (Just kidding - please don't actually use dance-offs or games of chance to resolve database conflicts.)

All of this happens on the client side, so you have full control over how your transactions are handled. And if a transaction can't be successfully committed after multiple attempts, the rollback mechanism kicks in to reverse any changes and return the database to a consistent state.

Overall, sequencers and resolvers work together to ensure that transactions in FoundationDB are ordered, consistent, and conflict-free. So go ahead, build something awesome on top of FoundationDB with confidence!

## References
[A distributed unbundled transaction store](https://www.foundationdb.org/files/fdb-paper.pdf)
