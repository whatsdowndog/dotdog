+++
title = "Choose Boring Technology Your Team Can Operate"
date = 2026-02-05
draft = true
+++

> **tl;dr:** The best technical choice is the one the team that inherits the system can actually run. If they can't operate it, it doesn't matter how good it is.

## the tempting option

- building a Transactions API for a large European retailer. core problem: querying and searching transaction data
- ElasticSearch was the obvious choice. purpose-built for this. full-text search, flexible querying, out-of-the-box filtering
- so why didn't we use it?
- no managed solution in the client's environment. someone would need to operate the cluster - upgrades, indices, split-brain, JVM heap tuning
- client team didn't have that operational experience, and we had ~1 year before full handover

## "choose boring technology" - but for whom?

- Dan McKinley's advice is usually framed around *your* team. what does your org know? what can your on-call debug at 3am?
- but when you're a consultancy building for handover, the question shifts: **boring for you might still be exotic for them**
- we could have operated ElasticSearch. done it before. but we wouldn't be the ones paged six months after leaving
- the people who'd be paged were the client engineers we were pairing with - smart, capable, but experienced with Postgres, not distributed search engines
- the decision wasn't about what we could run. it was about what they could run

## what we chose instead

- Postgres. boring, reliable, well-understood. client team already operated it confidently
- bypassed the ORM, wrote SQL by hand. ~dozen queries total. full control over performance, readable, directly tunable
- when you only have a handful of queries, ORM convenience isn't worth the abstraction
- **simple is not easy.** choosing Postgres meant solving filtering problems in application code that ElasticSearch would have handled for us. costs effort up front to save pain later

## the handover test

- heuristic: **can you explain this decision in 5 minutes to the person who'll operate the system after you leave?**
- "we use Postgres. here are the queries. they do what they look like they do." = 5 minutes
- "we run an ElasticSearch cluster, here's indexing, replication, what to do when a node drops, how to reindex without downtime" = a workshop
- client engineers we paired with daily were an ongoing test for this
- if we couldn't explain a choice clearly while pairing, that was a signal

## deferring is a valid architectural decision

- didn't reject ElasticSearch forever. deferred it
- at the time: no use case that couldn't be handled by Postgres + client-side filtering
- if that changed (data volumes grew, search requirements got sophisticated, managed option became available) - worth revisiting
- by that point the team would have more operational maturity and could make the call themselves
- **the best time to adopt complex technology is when you have both the use case and the team to support it**
- the system shipped on time, performed well, client team took ownership comfortably

*We help teams make technology choices that outlast our engagement. If you're wrestling with a build-vs-buy decision or a technology choice that has to survive a handover, [let's talk](mailto:hello@whatsdown.dog).*
