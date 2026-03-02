+++
title = "The Seven-Week Release Cycle (and How to Kill It)"
date = 2026-03-04
draft = true
+++

**TL;DR:** A fintech went from releasing every 7 weeks to deploying on demand. The fix wasn't a big-bang CI/CD migration - it was a sequence of small, boring steps, each unlocking the next.

## the starting point

- 7-week release cycle: 3 weeks dev, 3-4 weeks QA, deployment requires majority of the tech team working synchronously before normal working hours
- major releases: 50+ separate items per release
- QA period effectively a development "freeze" - teams juggling multiple branches and versioned environments
- smaller changes (app copy, email templates) coupled to the 7-week cycle - classified as "urgent" to release sooner
- some teams already working at a different cadence on less coupled services - and finding life much simpler

## why it was 7 weeks

- not one big problem, several interlocking ones:
  - **manual handoffs**: refreshing QA data, deploying versions, DB migrations, investigating outages - all required handing off to another team
  - **long feedback loops**: Jenkins builds 45-60 min, manual QA testing up to 3 weeks, automated regression suite 2-3 weeks to run
  - **context switching**: by the time rework happens, focus has moved on. rework interrupts current context or waits in a queue
  - **QA environment instability**: any issue with QA environments causes slowdowns. issues escalated manually through a chain of teams
  - **combinatorial testing complexity**: n features x m orgs, with unexpected breakages across subsets of orgs
- each problem made the others worse. long builds meant slow feedback. slow feedback meant context switches. context switches meant more bugs. more bugs meant longer QA cycles.

## the adjacent possible

- used an "adjacent possible" model: each step enables the next. you can't skip ahead
- items on the left are concrete and specific. items on the right are more conceptual - increased uncertainty over longer time horizons
- forms a dependency graph - completion of items on the left enables future capabilities on the right
- north star was full CI/CD, but we initially worked within the existing deployment model
- first target: **stable, quickly provisioned QA environments**
- from there: shift testing strategies, policy, development techniques
- then: full CI/CD to QA and staging
- finally: consider what still needs to change for full CI/CD to production

## what we actually did (phase 1)

- shipped Jenkins metrics first - immediately learned things: fragmented pipelines, unclear job ownership, scheduled cron builds, jobs pinned to specific workers
- parallelised Maven builds: one key repo went from 35-45 min to under 15 min
- helped teams publish intermediate artifacts to S3 so services only build their own code
- migrated inline Jenkins pipelines to source control - changes now tracked through commits and PRs
- switched from scheduled triggers to push/merge triggers
- discovered worker node drift/inconsistency - no standardised way to keep workers in sync

## what we actually did (phase 2)

- consolidated ~800 Jenkins jobs towards 1 pipeline per application repo
- moved common tasks (deploy, git tags) into shared scripts instead of embedded in every pipeline
- enabled merge gates: tests must pass, code quality addressed, coverage met before merge
- brought server configuration under code via Ansible
- moved towards trunk-based development

## the "months to minutes" outcome

- delivery cycle time went from 7 weeks to continuous, on demand
- not a big bang - an incremental pathway where each step measurably reduced cycle time
- each phase built on the previous one. couldn't have done phase 2 without phase 1 being solid
- teams that were previously frozen for 3-4 weeks of QA could now ship continuously

## lessons

- the release cycle wasn't the problem. it was a symptom of accumulated manual processes, missing automation, and environments nobody trusted
- you don't fix a 7-week cycle by "doing CI/CD". you fix it by identifying the bottlenecks in order and addressing them one at a time
- the adjacent possible model forced us to be honest about what we could actually do next, rather than what we wished we could do

*This is the engagement behind our "months to minutes" claim. If your release cycle is measured in weeks and you're not sure where to start, [get in touch](mailto:hello@whatsdown.dog).*
