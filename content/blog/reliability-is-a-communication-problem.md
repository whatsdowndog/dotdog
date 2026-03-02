+++
title = "Reliability is a Communication Problem"
date = 2026-01-28
draft = true
+++

**TL;DR:** The biggest bottleneck in large-scale reliability work isn't technical - it's that the rest of the company thinks it's someone else's problem.

## "that's an infrastructure concern"

Five months out from the biggest revenue event of the year, I walked into a product team's sprint planning to talk about a scaling bottleneck their service would hit under BFCM traffic. The room looked at me like I'd wandered into the wrong meeting. "That sounds like an infrastructure concern."

It wasn't. The bottleneck was in their application code, and when it hit in November, it wouldn't be an infrastructure problem - it would be a "merchants can't process orders" problem. But in July, with BFCM still an abstraction, convincing a product manager to prioritise a reliability fix over their roadmap was a harder sell than actually writing the fix.

**Reliability work that crosses organisational boundaries will fail if it's framed as a single team's responsibility.** The technical fixes are the easy part. The hard part is getting people outside your org to recognise that this is also their problem - and to act on it before it's urgent.

## available is not the same as communicated

Most engineering organisations pride themselves on transparency. Open documents, public Slack channels, shared dashboards. The information is all there - you just have to go find it.

For day-to-day work, this pull-based culture is fine. For a cross-cutting reliability project with tight deadlines and high stakes? It's a trap.

There's a massive difference between making information available and actually communicating it. We had a situation where a critical escalation to the cloud provider - the kind that needed CTO involvement - only happened because the CTO happened to read a weekly report. Not because anyone flagged it. Not because there was a process to surface risks of that magnitude. He just happened to be reading that week.

That's luck, not communication.

When you're running a project that touches every corner of the company, you can't rely on people pulling information to themselves. You have to push. You have to be annoyingly specific about what you need, from whom, and by when. If it feels like you're over-communicating, you're probably just communicating enough.

## getting exec buy-in (and getting it early)

One of the things we needed was deployment freezes during scale tests. Half a day, no deploys. Sounds reasonable when you're trying to isolate variables during a production load test. But freezing shipping requires CTO approval, and asking a CTO to stop the entire company from deploying - even for half a day - is a political ask as much as a technical one.

We won that argument, but only because we had data from a failed test where unrelated deployments caused incidents that paused the whole exercise. Without that evidence, we'd have been two engineers with an opinion.

**If you need executive buy-in, have the data before you make the ask.** And get in front of leadership early - not when you're already blocked. The deployment freeze conversation would have been easier if we'd socialised the idea weeks earlier, rather than coming in hot after a test went sideways.

## lightweight coordination that actually works

Heavy-weight programme management makes engineers miserable and doesn't work well for technical projects. But no coordination at all means you spend half your time figuring out who to talk to. We landed on a few patterns that struck the right balance:

- **Biweekly syncs with the cloud provider.** Structured, time-boxed, focused on capacity planning and upcoming tests. This was the single most valuable coordination mechanism we had.
- **Weekly office hours.** Anyone in the company could book a slot to discuss risks, ask questions, or flag concerns. Low barrier, surprisingly well-attended.
- **A GitHub repo for open risk triage.** Every known risk as an issue, triaged openly, visible to anyone. No status meetings required - just go read the issues.

None of this is revolutionary. That's the point. **The best coordination patterns are the ones boring enough that people actually use them.** A fancy project management setup that nobody updates is worse than a simple repo that everyone checks.

## build the cross-org team from day one

Here's the thing I'd do differently. Our core team was five people, all from the production engineering org. We were good at what we did, but we spent an embarrassing amount of time just figuring out who owned what in product and data science. There were no single points of contact. No embedded representatives. Every time we hit a problem that wasn't purely infrastructure, we were starting from scratch - mapping the org chart, finding the right person, building context from zero.

**If your reliability project touches multiple organisations, embed a representative from each one on day one.** Not as a liaison, not as someone who joins a weekly standup - as a proper member of the team who is empowered to make decisions and knows where the bodies are buried in their org.

This isn't about headcount. It's about having someone who can answer "who owns the recommendation engine's caching layer?" in five seconds instead of five days. The cost of one person's time is nothing compared to the cost of a cross-org project that can't find the right people to fix problems.

## so what do you actually do?

If you're about to kick off reliability work that spans your organisation, start here: assume nobody outside your team thinks this is their problem yet. Your first job isn't technical. It's making sure the right people understand why it matters, giving them a clear way to participate, and making it easy for risks to flow upward before they become incidents.

Reliability is a communication problem. Solve the communication first, and the engineering gets a lot easier.

*Cross-org reliability work is [a big part of what we do](/services). If your reliability project is stuck on organisational friction rather than technical problems, [we can help](mailto:hello@whatsdown.dog).*
