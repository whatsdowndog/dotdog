+++
title = "Failing Successfully: How to Run Performance Tests in Production"
date = 2026-01-14
draft = true
+++

**TL;DR:** A performance test that passes cleanly tells you almost nothing. A test that breaks things in a controlled way - that's the one that might save you on the worst day of the year.

## why you need to test in production

The first test was supposed to take four hours. Eight hours in, we were still debugging why the cloud provider's VM scheduler had gotten stuck, preventing new instances from being created in an entire region. This was not something staging would have told us.

When we ran scale tests for BFCM at a major e-commerce platform, we ran them in production - alongside real merchant traffic. That sounds terrifying, and it should. But staging doesn't surface hidden cloud provider quotas, cross-service dependencies that only matter at scale, or edge proxy behaviour under real load. You can't find production problems without production.

## split scale-up and performance testing

We broke the project into two distinct phases, and this turned out to be one of the best decisions we made.

**Scale-up testing** answers a simple question: can we even get to the right footprint? Can we provision enough capacity, scale our databases, warm our caches, and get every service to the size it needs to be? These are mostly infrastructure problems, and they tend to surface early.

**Performance testing** answers a different question: now that we're at the right size, can we handle the load? This requires realistic traffic simulations that take time to build, and it should happen closer to the event - after major features have shipped.

Mixing these up wastes time. If you're trying to diagnose a query performance issue while half your services are failing to scale, you'll spend hours chasing phantoms. Solve the infrastructure problems first, then stress-test the system at its intended size.

## scoping: what's in, what's out

Every team wanted to be included in the test by default. That's a natural instinct - nobody wants to be the service that wasn't tested and then falls over on Black Friday. But testing everything adds enormous coordination overhead, and most services aren't on the critical path.

We used a simple heuristic: **does this affect a merchant's ability to make money?** If a service going down means merchants can't process checkouts, it's in scope. If it means an internal dashboard loads slowly, it's out. This kept the test focused on the core platform, key secondary services, and critical vendor integrations - and kept us from trying to boil the ocean.

Saying no to teams who wanted to be included was uncomfortable. But a narrower scope meant faster tests, fewer coordination dependencies, and clearer results. You can always expand scope in later rounds once the critical path is solid.

## what "failing successfully" means

Here's the counterintuitive part: **a test that hits all its success criteria is the least useful test you can run.** It confirms what you already believed. A test that breaks - in a controlled, observable way - is the one that finds the problems you'd otherwise discover at 2am on Black Friday.

We called these "failing successfully." A test that exposed a scaling bottleneck in a data pipeline wasn't a failed test. It was a successful test that identified work to be done.

After each test, we wrote a report in the style of a post-incident retrospective. What happened, what broke, what the contributing factors were. Then a mini-retro with the broader team. Then remedial work - tracked centrally, with owners and deadlines. The cadence was: test, report, retro, fix, test again.

This framing matters. If a test that doesn't go perfectly is treated as a failure, teams will start optimising for clean test results instead of realistic test conditions. You want the opposite.

## the trap of optimism

We planned for two tests per phase - four total. We ended up running seven. Three scale-up tests. Four performance tests. Each one ran six to twelve hours.

The original plan assumed most issues would be on the infrastructure side and would be straightforward to remediate. In reality, scaling bottlenecks spanned product, data science, and infrastructure. Cloud provider capacity was constrained due to global supply chain issues. Fixes that looked simple in a retro turned out to be multi-week efforts.

**Plan for the number of tests you think you'll need, then double it.** Build a regular cadence - every two weeks - so there's always a next test coming. If a test goes well, great, you've validated your fixes. If it doesn't, you haven't blown your timeline.

Five months sounds like a lot. It wasn't.

## deployment freezes are worth the political cost

During our first test, teams kept deploying code to production. This is completely understandable - people have work to do - but it caused incidents that paused the test and muddied our results. We couldn't tell whether a problem was a genuine scaling issue or a bad deploy.

We requested a deployment freeze for test windows. This required CTO approval, because asking a large engineering organisation to stop shipping for half a day is a big ask. But the evidence from that first messy test made the argument for us. Subsequent tests ran dramatically more smoothly.

Think of it like a smoke alarm test in a building. You don't want people pulling the fire alarm for fun while you're trying to verify the system works. **A few hours of frozen deployments is a small price for results you can actually trust.**

## make it repeatable

The most important deliverable from the entire five-month project wasn't a successful test. It was documentation and tooling that meant the next team could own it without us.

Every test procedure was documented. Coordination patterns with the cloud provider were written down. Automation was built so that capacity requests, scaling operations, and result collection didn't require tribal knowledge.

The project retrospective identified something we should have recognised earlier: **monthly tests year-round beat a seasonal sprint every time.** Smaller, more frequent tests catch regressions early, keep the process fresh in people's minds, and turn BFCM preparation from a heroic effort into a boring operational habit.

That's the real goal. Not a perfect test, not a flawless Black Friday - but an organisation that treats large-scale reliability testing as just another thing it does. The best performance test is the one nobody writes a blog post about.

*This is the kind of work [we do at Downdog](/services). If you're staring down a peak traffic event and don't know where your system will break first, [we should talk](mailto:hello@whatsdown.dog).*
