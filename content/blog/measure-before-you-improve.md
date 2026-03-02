+++
title = "Measure Before You Improve"
date = 2026-03-12
draft = true
+++

**TL;DR:** Everyone at the company agreed Jenkins was a bottleneck. When we measured, the picture was more complicated - and the real problems were ones nobody had mentioned.

## the consensus diagnosis

- discovery phase: spoke to 32 people over two days
- "Jenkins is a bottleneck" came up repeatedly. but when we tried to validate across teams, we heard mixed responses
- possible explanations: only a bottleneck for some teams. some teams had found workarounds. different development cadences, build strategies, project complexity
- the diagnosis was plausible but unverified. we didn't trust it until we had data

## what the data actually showed

- first thing shipped: Jenkins metrics to Datadog
- immediately learned things nobody had mentioned:
  - path to production fragmented across a large number of jobs
  - ownership of jobs unclear
  - only a handful of jobs had configuration held as code
  - many things (tests, builds, deploys to QA) running as scheduled cron jobs - not triggered by code changes
  - jobs pinned to specific worker nodes, not sharing capacity - combined with scheduling, caused queuing
- the problem wasn't just "Jenkins is slow" - it was "the entire CI pipeline is fragmented, unowned, and doing unnecessary work"

## the observability gap

- teams using inbound/outbound request logs as primary observability. full request bodies for every interaction
- log retention: 15 days. metric retention: 15 months. no long-term trends
- Datadog APM already paid for but not configured to surface useful information
- one team had built dashboards from logs - good instinct, wrong tool for the job
- couldn't aggregate metrics across services. couldn't see error rates or latency over time. no alerting

## the QA blind spot

- "we asked QA to bring us specific examples of environmental problems. haven't had anything reported yet"
- reluctant to increase observability spend without a specific question to answer
- QA environment instability was described as the top issue slowing development - but nobody was monitoring QA environments
- issues discovered manually, escalated through a chain of teams
- no health checks, no automated notifications

## the pattern

- same pattern three times: people described a problem, we measured, and the real issue was different (or deeper) than the description
- Jenkins: "it's slow" → it's fragmented, unowned, and doing redundant work
- observability: "we use Datadog" → logs only, short retention, no aggregation, APM unconfigured
- QA stability: "it's the top issue" → no monitoring, no data, no reported examples
- **don't assume the diagnosis is correct just because everyone agrees on it**
- measure first, then prioritise based on what the data shows, not what people say

*Figuring out what's actually wrong before trying to fix it is [how we start every engagement](/services). If your team has a theory about what's slowing them down but no data to back it up, [we can help](mailto:hello@whatsdown.dog).*
