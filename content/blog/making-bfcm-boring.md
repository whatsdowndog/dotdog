+++
title = "Making BFCM Boring: Turning a Heroic Effort into a Repeatable Process"
date = 2026-02-26
draft = true
+++

**TL;DR:** The first year of large-scale reliability testing is a five-month, cross-company heroic effort. The goal is to make sure the second year isn't.

## the hero problem

It's June. Black Friday is five months away. I've just been handed ownership of making sure the platform doesn't fall over during the biggest revenue event of the year. The team is five people, all from production engineering. The problem spans product, data science, support, and multiple external vendors. Over the next five months: seven tests, countless remedial tasks, everyone working nights and weekends.

It worked. The platform stayed up. People were congratulated. And that's exactly the problem.

**Heroic efforts feel good, but they don't scale.** If your BFCM preparation requires the same five people to pull the same all-hands effort every year, you haven't built a process - you've built a dependency on specific humans being willing to sacrifice their autumn. That's not reliability engineering. That's luck with extra steps.

## document everything, even when it's painful

After each of those seven tests, we wrote a report in the style of a post-incident retrospective. What broke, what surprised us, what we'd do differently. Mini-retrospectives after each cycle. Centralised tracking in a GitHub repo so every remedial task had an owner and a deadline.

Here's the thing nobody wants to hear: **the documentation wasn't for us.** We already knew what happened - we were there. The documentation was for the team that would own it next year, the team that wasn't in the room yet.

Writing things down when you're exhausted and just want to move on to the next test feels wasteful. It isn't. Future-you - or more likely, future-someone-else - will have no idea why a particular cloud provider quota matters or why a specific service needs to be scaled up 48 hours before the event rather than 24. If it's not written down, it's not knowledge. It's trivia trapped in someone's head.

## build tooling, not just process

A runbook that says "coordinate capacity with the cloud provider, then verify all remedial fixes have landed, then run the test, then check results" is better than nothing. But it still requires someone to follow the steps manually, in the right order, without missing anything.

**Process is fragile. Tooling is durable.** We built automation around the scale tests themselves - provisioning, verification, reporting. Not because we were trying to be clever, but because manual coordination across that many teams and vendors is where mistakes live. Every manual step is a step someone will eventually skip or get wrong, especially at 11pm the night before a test.

The distinction matters. A process document can go stale the moment someone changes a configuration. A tool that actually runs the steps will fail loudly when something changes - and that failure is a signal, not a disaster.

## the real deliverable: organisational capability

Here's how I know the project actually worked: my team was able to own BFCM preparations more fully the following year, after I'd left. They didn't need to call me. They didn't need to reverse-engineer what we'd done. The documentation and tooling were sufficient for a team that had been involved - but not leading - the first time around.

That's the real deliverable. Not a successful test, not a green dashboard on Black Friday. **The real deliverable is organisational capability** - the ability for the company to do this without any specific individual.

Think of it like installing a fire alarm. The first time there's a fire, someone heroically notices the smoke and gets everyone out. The second time, you want a system that works whether that person is in the building or not.

## test smaller, test more often

The project retrospective surfaced something we'd suspected but hadn't acted on: many of the issues we found didn't actually require scaling the entire system to reproduce. A service hitting a database connection limit, a queue consumer falling behind under load - these could have been found by testing smaller parts in isolation.

Scaling the whole platform for a test is expensive. It requires coordination across dozens of teams, deployment freezes, and vendor involvement. It's a whole-day event. **If you can find the same bug by testing one service in isolation, you've saved yourself an enormous amount of coordination overhead.**

The retrospective recommended monthly tests year-round instead of cramming everything into a few months before the event. Smaller scope, higher frequency. Turn BFCM preparation from a seasonal sprint into a continuous activity. The issues don't wait until October to appear - they accumulate quietly all year as teams ship features and change configurations.

## you're done when the next team can do it without you

So what do you actually do? You run the heroic effort the first time - there's no shortcut around that. But while you're doing it, you write everything down, you build tools to replace manual steps, and you treat the whole thing as a project that needs to survive your departure.

The goal isn't to run the best test. The goal isn't even to keep the site up on Black Friday - though obviously that matters. **The goal is to make the next time boring.** Predictable. Routine. The kind of thing someone can own without needing a five-month heads-up and a dedicated war room.

You know you've succeeded when nobody writes a blog post about it.

*Turning heroic efforts into repeatable processes is [what we do](/services). If your team is running on adrenaline instead of systems, [we should talk](mailto:hello@whatsdown.dog).*
