+++
title = "800 Jenkins Jobs"
date = 2026-03-26
+++

**TL;DR:** Nobody sets out to have 800 Jenkins jobs. It happens one "just copy this pipeline and tweak it" at a time. CI/CD rot is real, and it compounds silently. Using tools "with the grain" can massively simplify operations and lead to surprising benefits.

## what happened

At Downdog, we both suffer from chronic SRE-brain. Some say it might even be [terminal](https://ghostty.org/). One symptom of this affliction is a deep distrust of messiness - whether that is tool sprawl, lack of clear ownership, unfocussed dashboards, or deeply-nested `if` statements. 

 Some moons ago, we saw something at a client that bothered us: a Jenkins instance with over 800 distinct jobs, mostly configured directly through the UI. Considering the client had fewer than 20 live services, this felt off. No one we spoke to could quite explain why that number was so high, but did share stories of how flaky, slow, and impenetrable the whole setup was.

 We decided to investigate and dig deeper. We found:

- most repos had at least 5 jobs: PR validation, build, deploy to QA, deploy to prod, etc. These jobs would call each other in succession, which got the job done (...most of the time), but gave zero visibility into how far through the chain anything was
- repositories containing multiple services saw job counts multiply exponentially. One repo had 50+ related jobs. We affectionately named these micro-mono-repos - multiple services crammed into one repo without the tooling to support a true monorepo
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

Our favoured approach is to help clients get the most of their existing tools, as fast as possible, and set them up for a smoother migration should they choose to embark on that journey later. This also means working "with the grain" of their tools - in other words, using established patterns recommended by the tool authors and community, rather than shoehorning or duct-taping tools into doing things they are not well-suited for.

In terms of Jenkins specifically, this meant:
- one Jenkinsfile pipeline per application repository, leveraging common scripts for standard tasks. This enabled usage of Jenkins' org folder feature, meaning that new pipelines are automatically detected, reducing the amount of time teams had to spend configuring Jenkins by hand
- moving project-specific logic into configuration (e.g. top-level `pom.xml`) so pipelines were no longer coupled to repo structure
- larger tasks (e.g. deploying) moved into application-agnostic scripts pulled from a shared repository. This  clearly separated what is owned by DevOps (infrastructure/deployment) from what is owned by dev teams (build/test/code analysis), and makes future migration between CI/CD tools easier
- migrating all inline pipelines to code - changes tracked through commits and PRs, rather than through Jenkins' awkward XML diffing
- one "shape" of pipeline for each kind of repo: backend, frontend, infrastructure. We wrote a small tool to template these so that they would stay consistent over time, and allow the client's DevOps team to roll out changes across all pipelines easily

We shamelessly ~cribbed~ were inspired by the Jenkins open source org's [own Jenkins setup](https://ci.jenkins.io/), which taught us a lot about how the Jenkins team thinks about Jenkins! We highly recommend this trick for any tool, as it often offers insight into its future direction.

We ended up with a massively simplified setup:
* 800 jobs reduced to ~40 pipelines - one per active repo
* everything backed up fully as code, most pipelines generated from established templates
* pipelines ran significantly faster (due to jobs no longer sitting in queues)
* pipelines could be understood by any engineer at the company
* data teams started using Jenkinsfiles and shared CI/CD practices

We also received feedback that teams were no longer afraid of CI (!) and felt empowered to spin up new pipelines themselves, following the established patterns.

## the lesson

CI/CD rot is real. every shortcut, every copy-paste, every "just add a new job" adds maintenance burden that compounds silently. You don't notice it accumulating because each individual change is small and reasonable, and by the time you notice, you have 800 jobs and nobody can explain why half of them exist. The fix is simple and boring engineering: shared patterns, config as code, clear ownership, and the willingness to delete things:
**if nobody can explain what a pipeline does, it probably shouldn't exist**

*CI/CD setups that nobody understands are a reliability risk. If your build system has grown beyond anyone's ability to reason about it, we've seen this pattern before and can help untangle it - [get in touch!](mailto:hello@whatsdown.dog)*
