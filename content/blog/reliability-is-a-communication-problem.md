+++
title = "Reliability is a Communication Problem"
date = 2026-01-28
draft = true
+++

**TL;DR:** The biggest bottleneck in large-scale reliability work isn't technical - it's that the rest of the company thinks it's someone else's problem.

## "that's an infrastructure concern"

- five months before BFCM, walked into a product team's sprint planning to flag a scaling bottleneck in their service
- the room looked at me like I'd wandered into the wrong meeting. "that sounds like an infrastructure concern"
- it wasn't. bottleneck was in their application code. in November it would be a "merchants can't process orders" problem
- but in July, with BFCM still abstract, convincing a PM to prioritise a reliability fix over their roadmap was harder than writing the fix itself
- in previous years, BFCM prep was handled entirely by production engineering. product teams saw it as not their problem
- **reliability work that crosses org boundaries will fail if framed as one team's responsibility**

## available is not the same as communicated

- most engineering orgs pride themselves on transparency. open docs, public Slack channels, shared dashboards
- pull-based culture works for day-to-day. for cross-cutting reliability with tight deadlines? it's a trap
- a critical CTO escalation to the cloud provider only happened because the CTO happened to read a weekly report. not flagged. no process to surface risks of that magnitude. luck, not communication
- when running a project that touches every corner of the company, you have to push. annoyingly specific about what you need, from whom, by when
- if it feels like over-communicating, you're probably just communicating enough

## getting exec buy-in (and getting it early)

- needed deployment freezes during scale tests. half a day, no deploys
- sounds reasonable when isolating variables in a production load test
- but freezing shipping requires CTO approval - political ask as much as technical
- won the argument because we had data from a failed test where unrelated deploys caused incidents
- without that evidence: just two engineers with an opinion
- **have the data before you make the ask.** get in front of leadership early, not when you're already blocked

## lightweight coordination that actually works

- heavyweight programme management makes engineers miserable. no coordination = half your time figuring out who to talk to
- what worked:
  - biweekly syncs with cloud provider - structured, time-boxed, focused on capacity. single most valuable mechanism
  - weekly office hours - anyone could book a slot. low barrier, surprisingly well-attended
  - GitHub repo for risk triage - every known risk as an issue, triaged openly, no status meetings needed
- **the best coordination patterns are the ones boring enough that people actually use them**

## build the cross-org team from day one

- the thing I'd do differently
- core team was 5 people, all from production engineering
- spent embarrassing amount of time figuring out who owned what in product and data science
- no single points of contact. no embedded reps. every non-infra problem = starting from scratch, mapping the org chart
- **embed a representative from each affected org on day one.** not a liaison, not someone who joins a weekly standup - a proper team member empowered to make decisions
- cost of one person's time is nothing vs. a cross-org project that can't find the right people

## so what do you actually do?

- assume nobody outside your team thinks this is their problem yet
- your first job isn't technical - it's making sure the right people understand why it matters
- give them a clear way to participate, make it easy for risks to flow upward before they become incidents
- reliability is a communication problem. solve the communication first, the engineering gets easier

*Cross-org reliability work is [a big part of what we do](/services). If your reliability project is stuck on organisational friction rather than technical problems, [we can help](mailto:hello@whatsdown.dog).*
