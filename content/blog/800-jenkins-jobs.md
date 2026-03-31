+++
title = "800 Jenkins Jobs"
date = 2026-03-26
+++

**TL;DR:** Nobody sets out to have 800 Jenkins jobs. It happens one "just copy this pipeline and tweak it" at a time. CI/CD rot is real, and it compounds silently. Using tools "with the grain" can massively simplify operations and lead to surprising benefits.

## what happened

At Downdog, we both suffer from chronic SRE-brain. Some say it might even be [terminal](https://ghostty.org/). One symptom of this affliction is a deep distrust of messiness - whether that is tool sprawl, lack of clear ownership, unfocussed dashboards, or deeply-nested `if` statements. 

 Some moons ago, we saw something at a client that bothered us: a Jenkins instance with over 800 distinct jobs, mostly configured directly through the UI. Considering that the client had fewer than 20 live services, this felt off. No-one we spoke to could quite explain why that number was so high, but did share stories of how flaky, slow, and impenetrable the whole setup was.

 We decided to investigate and dig deeper. We found:

- most repos had at least 5 jobs: PR validation, build, deploy to QA, deploy to prod, etc. These jobs would call each other in succession, which got the job done (...most of the time), but gave zero visibility into how far through the chain anything was
- repos with multiple services within multiplied this per service. One repo had 50+ related jobs. We affectionately named these micro-mono-repos
- common tasks (git tagging, deployments) were embedded directly in pipeline scripts, often as Bash-in-a-textbox 
- keeping equivalent tasks consistent required finding and updating all relevant pipelines manually through the UI, resulting in a significant amount of drift 
- scripts were deeply coupled to repository structure, and behaved differently depending on existence of particular directories or build artifacts

## understanding how it happens

Nobody plans for this to happen. Teams are rushed, patterns aren't established, and information disseminates organically: 

> "copy the pipeline from that other project and tweak it"

> "add a new job for this deployment target"

> "this repo has a different structure, so the script needs a special case"

> "we need a separate job for the release branch"

Each micro-decision unblocks delivery in the moment, and keeps everything moving for that hour or day. After a while, things start grinding to a halt - similar to the way rushing application code results in new features eventually becoming impossible. 

## how we fixed it

You may be thinking: "why don't you _just_ use GitHub Actions/Buildkite/the latest Kubernetes-native hotness?"

And yes, when things are a mess, one way out is to rebuild from scratch, cleanly. Unfortunately, in our experience: 
* engagements are already extremely time constrained
* we are usually brought in to help clients achieve a specific business-related goal, and most of the time our business sponsors aren't particularly interested in how we achieve the goal - just that we achieved it
* tool migrations only become net-positive when 100% complete - otherwise you're now running two systems in parallel and trying to keep them in sync
* the last 10% of a migration always take longer than anyone expects, even when you take this fact into account (see [Hofstadter's law](https://en.wikipedia.org/wiki/Hofstadter%27s_law))

- one pipeline per application repository, leveraging common scripts for standard tasks
- moved project-specific logic into configuration (e.g. top-level pom.xml) so pipelines less coupled to repo structure
- larger tasks (deploying) moved into application-agnostic scripts in shared repos
- clearly separated what should be owned by DevOps (infrastructure/deployment) from what should be owned by dev teams (build/test/code analysis)
- migrated all inline pipelines to code - changes tracked through commits and PRs
- switched from cron triggers to push/merge triggers
- one "shape" of pipeline per service type: Spring Boot, JAR/WAR, front-end

## the lesson

- CI/CD rot is real. every shortcut, every copy-paste, every "just add a new job" adds maintenance burden that compounds silently
- you don't notice it accumulating because each individual change is small and reasonable
- by the time you notice, you have 800 jobs and nobody can explain why half of them exist
- the fix isn't heroic - it's disciplined: shared patterns, config as code, clear ownership, and the willingness to delete things
- **if nobody can explain what a pipeline does, it probably shouldn't exist**

*CI/CD setups that nobody understands are a reliability risk. If your build system has grown beyond anyone's ability to reason about it, [we can help untangle it](mailto:hello@whatsdown.dog).*
