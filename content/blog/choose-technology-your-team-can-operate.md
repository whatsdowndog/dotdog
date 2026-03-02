+++
title = "Choose Technology Your Team Can Operate"
date = 2026-02-05
draft = true
+++

**TL;DR:** The best technical choice is the one the team that inherits the system can actually run. If they can't operate it, it doesn't matter how good it is.

## the tempting option

ElasticSearch was - on paper - the right tool for the job. We were building a Transactions API for a large European retailer, and the core problem was querying and searching through transaction data. ElasticSearch is literally designed for this. It would have given us powerful full-text search, flexible querying, and the kind of out-of-the-box filtering that would take real effort to replicate elsewhere.

So why didn't we use it?

There was no managed solution available in the client's environment. That meant someone would need to operate an ElasticSearch cluster - handle upgrades, manage indices, deal with split-brain scenarios, tune JVM heap sizes. The client team didn't have that operational experience, and we weren't going to be around long enough to build it for them. We had roughly a year before the entire system would be handed off.

## "choose boring technology" - but for whom?

Dan McKinley's famous advice to choose boring technology is usually framed around *your* team. What does your organisation already know? What can your on-call engineers debug at 3am? That's good advice. But when you're a consultancy building something that will be handed off, the question shifts. **Boring for you might still be exotic for them.**

Our team could probably have operated ElasticSearch. We'd done it before. But we weren't the ones who'd be paged when something went wrong six months after we left. The people who would be paged were the client engineers we were pairing with - engineers who were smart and capable, but whose operational experience was with Postgres, not distributed search engines.

The technology decision wasn't about what we could run. It was about what they could run.

## what we chose instead

Postgres. Boring, reliable, well-understood Postgres that the client team already operated confidently.

We also bypassed the ORM and wrote SQL by hand. The number of queries in the system was small - maybe a dozen or so - and we wanted full control over query performance rather than hoping the ORM would generate something sensible. When you only have a handful of queries, the convenience of an ORM isn't worth the abstraction. You can read the SQL, understand exactly what it does, and tune it directly.

**Simple is not easy.** Writing raw SQL for a small set of queries takes more discipline than letting an ORM do the work. Choosing Postgres over ElasticSearch meant we had to solve filtering problems in application code and on the client side - work that ElasticSearch would have handled for us. Simplicity isn't the path of least resistance. It's a constraint you enforce deliberately, and it costs effort up front to save pain later.

## the handover test

Here's the heuristic we kept coming back to: **can you explain this decision in five minutes to the person who'll be operating the system after you leave?**

"We use Postgres. Here are the queries. They do what they look like they do." That fits in five minutes. "We run an ElasticSearch cluster - here's how indexing works, here's the replication model, here's what to do when a node drops out of the cluster, here's how to reindex without downtime" - that's a workshop, not a handover.

If your architecture decision requires a paragraph of caveats before someone can operate it safely, it's probably the wrong decision for a system that needs to survive your departure. The client engineers we were pairing with every day were an ongoing test for this. If we couldn't explain a choice to them clearly while pairing, that was a signal.

## deferring is a valid architectural decision

We didn't reject ElasticSearch forever. We deferred it. There's a difference, and it matters.

At the time we made the decision, there was no use case that couldn't be handled by Postgres queries and client-side filtering. If that changed - if the data volumes grew, if search requirements got more sophisticated, if a managed ElasticSearch option became available - then it would absolutely be worth revisiting. And by that point, the team operating the system would have more operational maturity and could make that call themselves.

Deferring a technology choice until there's a clear need is not a failure of ambition. It's recognising that **the best time to adopt a complex technology is when you have both the use case and the team to support it** - and not a moment before.

The system shipped on time, performed well, and the client team took ownership of it comfortably. No heroes required.

*We help teams make technology choices that outlast our engagement. If you're wrestling with a build-vs-buy decision or a technology choice that has to survive a handover, [let's talk](mailto:hello@whatsdown.dog).*
