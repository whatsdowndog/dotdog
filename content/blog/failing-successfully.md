+++
title = "Task Failed Successfully: How to Run Performance Tests in Prod"
date = 2026-01-14
draft = true
+++

> **tl;dr:** A performance test that passes cleanly tells you almost nothing. A test that breaks things in a controlled way - that's the one that might save you on the worst day of the year.

## why you need to test in production

- first scale test: load ramped up, overwhelmed edge vendor, got rate-limited, had to stop
- we'd assumed we could throw load from a single location. couldn't.
- load generation had to be re-engineered to spread across multiple POPs
- none of this would have shown up in staging
- staging doesn't have your real edge vendor config, your real cross-service dependencies, your real cloud provider quotas
- ran tests in production alongside real merchant traffic

## split scale-up and performance testing

- two distinct phases, one of the best decisions we made
- scale-up: can we even get to the right footprint? provision capacity, scale DBs, get every service to BFCM size. mostly infrastructure problems.
- performance: now that we're at the right size, can we handle the load? needs realistic traffic simulations, should happen closer to the event after major features have shipped
- don't mix these up - if you're debugging query performance while half your services can't scale, you're chasing phantoms
- tradeoff: performance tests need to be close to the event, but you need to start early. splitting the phases resolves this.

## scoping: what's in, what's out

- every team wanted to be included. natural instinct - nobody wants to be the untested service that falls over
- but testing everything = enormous coordination overhead
- heuristic: **does this affect a merchant's ability to make money?**
- in scope: core platform, ~6 secondary services, 3 key vendors (payments, observability, tax)
- out of scope: everything else. saying no was uncomfortable but necessary
- can always expand scope in later rounds once critical path is solid

## what "failing successfully" means

- counterintuitive: a test that passes cleanly is the least useful test
- a test that breaks in a controlled way finds the problems you'd otherwise discover during the real event
- we called this "failing successfully"
- after each test: report (post-incident retro style), mini-retro with broader team, remedial work tracked centrally with owners and deadlines
- cadence: test, report, retro, fix, test again
- framing matters: if a broken test is treated as a failure, teams optimise for clean results instead of realistic conditions

## the trap of optimism

- planned for 2 tests per phase (4 total). ran 7 (3 scale-up, 4 performance)
- original plan assumed issues would be mostly infrastructure and straightforward to fix
- reality: bottlenecks spanned product, data science, infrastructure
- cloud provider capacity constrained (global supply chain issues, 2021)
- fixes that looked simple in retro turned out to be multi-week efforts
- built a cadence of every two weeks - always a next test coming
- five months sounds like a lot. it wasn't.

## deployment freezes are worth the political cost

- first test: teams kept deploying, caused incidents, paused the test
- couldn't tell if problems were scaling issues or bad deploys
- requested deployment freeze for test windows. required CTO approval.
- evidence from the first messy test made the argument
- subsequent tests ran much more smoothly
- big ask politically, but small cost vs untrustworthy test results

## make it repeatable

- most important deliverable wasn't a successful test - it was documentation and tooling so the next team could own it
- every procedure documented, coordination patterns written down, automation built
- the colleague I trained up ran BFCM the following year without me, and wrote the public blog post about it
- retrospective conclusion: monthly tests year-round beat a seasonal sprint
- goal: make large-scale testing boring and routine, not heroic

*This is the kind of work [we do at Downdog](/services). If you're staring down a peak traffic event and don't know where your system will break first, [we should talk](mailto:hello@whatsdown.dog).*
