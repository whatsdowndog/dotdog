+++
title = "Treat QA Like Production (or Better Yet, Kill It)"
date = 2026-03-17
draft = true
+++

**TL;DR:** QA environment instability was the number one thing slowing down development. Nobody was monitoring QA. If the environments that gate your development process are unreliable, nothing downstream can be reliable either.

## "almost as important as production"

- from the discovery report: "QA is almost as important as production, but without the same level of monitoring"
- QA environment instability was described by every team as one of the top issues slowing development
- but QA had none of the monitoring, alerting, or health checks that production had
- issues discovered manually by QA testers, then escalated through a chain of teams in a "pull" direction
- nobody was automatically detecting and routing problems ("push" direction)

## what instability actually looked like

- significant variation in how QA boxes were refreshed, maintained, and populated with data
- mismatches in database schema, configuration, and access to 3rd party integrations
- resolving versioning mismatches took hours to days
- refreshing data in QA required a manual handoff to another team - completed "in a timely manner, once picked up" but the wait and context switch added up
- deploying specific versions to a QA box: also a manual handoff

## the vicious cycle

- increasing deployment frequency to QA without first addressing instability would make things worse, not better
- more frequent deploys to an unstable environment = more frequent breakages = more escalations = more slowdowns
- you have to stabilise the environment before you can increase throughput
- this was the key insight that shaped the engagement: the first step towards CI/CD was not CI/CD. it was making QA environments stable and quickly provisioned

## applying DORA metrics to QA

- measured QA environments the same way you'd measure production:
  - how long to provision a QA environment (including approvals and manual steps)
  - how frequently QA environments break
  - how long to fix broken QA environments
- these correlate to DORA metrics deliberately
- the goal: trust that increasing deployment frequency to QA will neither compromise stability nor increase team workloads

## what we did

- service health checks on QA environments - flag issues to the relevant team at the point of breakage
- observability improvements: same monitoring in QA as production
- automated database updates: removed manual handoffs for schema changes
- automated environment provisioning: VMs provisioned quickly and automatically, not manually
- push-based notifications instead of pull-based escalation chains

## the real lesson: kill staging

- note: a better solution to unstable staging/QA environments is to not have them at all
- the more environments you have between a developer's machine and production, the more things can go wrong
- each environment is a maintenance burden, a source of drift, and a place where time is lost
- the ideal: deploy to production behind feature flags, with good observability and the ability to roll back quickly
- staging environments exist because of a lack of confidence in automated testing and deployment. fix the confidence problem and you can eliminate the environments
- we couldn't do this at this client - the 7-week release cycle and manual QA process meant there was no foundation for it. but the end state should be: trunk-based development, continuous deployment to production, no staging
- staging is a crutch. the goal is to not need it

*If your development process is gated by environments nobody trusts, [we can help you fix that](/services) - or help you figure out how to eliminate them entirely. [Get in touch](mailto:hello@whatsdown.dog).*
