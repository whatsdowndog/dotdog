+++
title = "Cache Fools Everyone Around Me"
date = 2026-02-09
draft = true
+++

> **tl;dr:**
> Adding a cache doesn't automatically make things faster. If the cache is slower than the database, you've just added latency and complexity for nothing.

## the reflex

- something is slow. someone says "let's put a cache in front of it." everyone nods
- engineering reflex that feels so obviously correct nobody questions it
- we were building a transactions API. performance requirements: 350 req/s at <100ms latency. hard targets
- someone suggested Elasticache

## the surprise

- **Elasticache on a bigger network hop was slower than hitting Postgres directly**
- database was close. data set was small. queries were simple
- adding a cache = adding a network round trip longer than just asking Postgres
- sounds obvious in hindsight, genuinely counterintuitive in the moment
- "database bad, cache good" is a heuristic, not a rule. heuristics break when assumptions change

## other performance assumptions worth questioning

- **ORMs**: bypassed ours, wrote SQL by hand. number of queries was small enough to count. full control over what runs, readable, directly tunable. ORM would have given us an abstraction we didn't need and unpredictable performance
- **stored procedures**: the other extreme. "move logic closer to data." but gross and hard to test granularly. you've traded app complexity for DB complexity, and your database is now doing two jobs
- same pattern every time: **reaching for a "performance" solution without proving there's a performance problem, or that the solution helps**

## measure, don't guess

- what actually kept us fast: knowing at all times whether we were fast
- nightly performance tests, automatic. every morning: actual numbers, not feelings
- shorter perf test in CI on every commit. catch regressions before merge
- starting simple and fast from day one gave us headroom. protecting a fast system, not scrambling to optimise a slow one
- **if you're not measuring, you're guessing. your guesses about what's slow are probably wrong**

*Performance tuning is [one of the things we do](/services). If you're chasing latency problems or not sure where your bottlenecks are, [we'd love to help](mailto:hello@whatsdown.dog).*
