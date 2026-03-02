+++
title = "Caching Can Make You Slower"
date = 2026-02-09
draft = true
+++

**TL;DR:** Adding a cache doesn't automatically make things faster. If the cache is further away than the database, you've just added latency and complexity for nothing.

## the reflex

Something is slow. Someone on the team says "let's put a cache in front of it." Everyone nods. It's one of those engineering reflexes that feels so obviously correct that nobody questions it. Cache equals fast, right?

We were building a transactions API for a large European retailer. The performance requirements were brutal - 350 requests per second at under 100 milliseconds latency. Not "best effort," not "p99 under 200ms on a good day." Hard targets that the system had to hit or the project would fail.

So naturally, someone suggested Elasticache.

## the surprise

Here's what actually happened: **Elasticache, sitting on a bigger network hop, was slower than hitting Postgres directly.** The database was close. The data set was small. The queries were simple. Adding a cache meant adding a network round trip that was longer than just asking Postgres for the answer.

This is the kind of thing that sounds obvious in hindsight but is genuinely counterintuitive in the moment. We've all internalised "database bad, cache good" as a rule. But it's not a rule - it's a heuristic, and heuristics break when the underlying assumptions change. If your database is fast and local and your cache is slow and distant, the cache makes you slower. Full stop.

## other performance assumptions worth questioning

Caching isn't the only place where the "obvious" performance optimisation turns out to be wrong.

**ORMs.** We bypassed ours entirely and wrote SQL by hand. Sounds reckless? The number of queries was genuinely small - we could count them. For that kind of surface area, you get more from controlling exactly what SQL runs than you do from the convenience of an ORM. We could read each query, reason about its execution plan, and tune it precisely. An ORM would have given us an abstraction we didn't need and performance characteristics we couldn't easily predict.

**Stored procedures.** The other extreme. "Move the logic closer to the data and it'll be faster." Maybe. But stored procedures are gross and hard to test in a granular way. You've traded application complexity for database complexity, and your database is now doing two jobs instead of one. Unless you have a very specific, measured reason to go this route, don't.

The pattern here is the same every time: **someone reaches for a "performance" solution without first proving there's a performance problem, or that the solution actually helps.**

## measure, don't guess

The thing that actually kept us fast wasn't any single optimisation. It was knowing, at all times, whether we were fast.

We ran nightly performance tests automatically. Every night, the system got hit with realistic load, and every morning we had numbers. No surprises. No "I think it's probably fine." Actual data.

On top of that, we ran a shorter performance test in CI on every commit. Not the full nightly suite - just enough to catch obvious regressions before they merged. If your change made things slower, you found out in minutes, not days.

This is where the real leverage is. Starting simple and fast from day one gave us headroom. We weren't scrambling to optimise a system that was already too slow - we were protecting a system that was already fast enough. That's a much better position to be in.

## so what do you actually do?

**Start simple.** Don't add a cache, an ORM, or a stored procedure because it feels like the right thing. Start with the most straightforward approach and measure what actually happens.

**Measure continuously.** Nightly performance tests and CI-level checks aren't expensive to set up, and they pay for themselves the first time they catch a regression you wouldn't have noticed until production.

**Question the reflexes.** The next time someone says "it's slow, let's cache it" - ask where the cache lives, how far the hop is, and whether anyone has actually profiled the thing that's supposedly slow. The answer might surprise you.

Performance engineering is not about knowing the right answers. It's about being honest enough to admit you don't know - and then measuring until you do.

*Performance tuning is [one of the things we do](/services). If you're chasing latency problems or not sure where your bottlenecks are, [we'd love to help](mailto:hello@whatsdown.dog).*
