+++
title = "Making BFCM Boring: Scaling Process From One->Many"
date = 2026-02-26
draft = true
+++

**TL;DR:** The first year of large-scale reliability testing is a five-month, cross-company heroic effort. The goal is to make sure the second year isn't.

## the hero problem

- it's June. Black Friday is five months away. I've just been handed ownership of making sure the platform doesn't fall over during the biggest revenue event of the year
- team: five people, all from production engineering. problem spans: product, data science, support, multiple external vendors
- over the next five months: seven tests, countless remedial tasks, nights and weekends
- it worked. platform stayed up. people were congratulated. and that's exactly the problem
- **heroic efforts feel good but don't scale.** if BFCM prep requires the same five people pulling the same effort every year, you've built a dependency on specific humans, not a process
- that's luck with extra steps

## document everything, even when it's painful

- after each of seven tests: report in post-incident retro style. what broke, what surprised us, what we'd do differently
- mini-retrospectives after each cycle. centralised tracking in GitHub repo, every task with owner and deadline
- **the documentation wasn't for us.** we knew what happened - we were there. it was for the team that would own it next year
- writing things down when you're exhausted and want to move on feels wasteful. it isn't
- if it's not written down, it's not knowledge. it's trivia trapped in someone's head

## build tooling, not just process

- a runbook saying "coordinate capacity, verify fixes, run test, check results" is better than nothing
- but still requires someone to follow steps manually, in order, without missing anything
- **process is fragile. tooling is durable.**
- built automation around provisioning, verification, reporting
- every manual step = a step someone will eventually skip or get wrong
- a process doc goes stale when config changes. a tool that runs the steps fails loudly - that failure is a signal, not a disaster

## the real deliverable: organisational capability

- how I know the project worked: my team owned BFCM the following year, after I'd left. didn't need to call me
- the colleague I trained up went on to write the public blog post about the process
- documentation and tooling were sufficient for a team that had been involved but not leading the first time
- **the real deliverable is organisational capability** - the ability to do this without any specific individual
- fire alarm analogy: first fire, someone heroically notices smoke and gets everyone out. second time, you want a system that works whether that person is in the building or not

## test smaller, test more often

- retrospective surfaced: many issues didn't require scaling the entire system to reproduce
- service hitting a DB connection limit, queue consumer falling behind under load - testable in isolation
- scaling the whole platform = expensive. coordination across dozens of teams, deployment freezes, vendor involvement
- **if you can find the same bug testing one service in isolation, you've saved enormous coordination overhead**
- retrospective recommended monthly tests year-round instead of cramming into a few months
- smaller scope, higher frequency. issues accumulate quietly all year as teams ship features

## you're done when the next team can do it without you

- run the heroic effort the first time - no shortcut. but while doing it: write everything down, build tools, treat it as a project that needs to survive your departure
- **the goal is to make the next time boring.** predictable. routine. ownable without a five-month heads-up and a war room
- you know you've succeeded when nobody writes a blog post about it

*Turning heroic efforts into repeatable processes is [what we do](/services). If your team is running on adrenaline instead of systems, [we should talk](mailto:hello@whatsdown.dog).*
