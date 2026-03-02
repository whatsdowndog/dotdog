+++
title = "800 Jenkins Jobs and How We Got There"
date = 2026-03-26
draft = true
+++

**TL;DR:** Nobody sets out to have 800 Jenkins jobs. It happens one "just copy this pipeline and tweak it" at a time. CI/CD rot is real, and it compounds silently.

## the archaeology

- Propel had ~800 distinct Jenkins jobs
- most repos had at least 5 jobs: PR validation, build, deploy to QA, deploy to prod, etc.
- multi-app repos multiplied this per application. one repo (mk-shared-services) had 50+ related jobs
- common tasks (git tags, deployments) embedded directly in pipeline scripts - not shared
- keeping equivalent tasks consistent required finding and updating all relevant pipelines, accounting for drift that had already occurred
- scripts coupled to repository structure - behaved differently depending on existence of particular directories or build artifacts

## how it happens

- nobody plans for 800 jobs. it accumulates:
  - "copy the pipeline from that other project and tweak it"
  - "add a new job for this deployment target"
  - "this repo has a different structure, so the script needs a special case"
  - "we need a separate job for the release branch"
- each decision is reasonable in isolation. in aggregate: unmaintainable
- inline pipelines configured through the Jenkins UI - not easily manipulated as code, not tracked through version control
- jobs pinned to specific worker nodes rather than sharing capacity. combined with cron scheduling, this caused jobs to queue instead of executing immediately
- worker nodes had version/tooling drift with no standardised way to keep them in sync

## what we found along the way

- implicit dependencies between builds that weren't documented
- complexity in local builds on developer machines
- for many repos, cutting a new release branch required manually setting up new, release-specific pipelines - without which integration jobs would fail
- services with internal dependencies relied on those dependencies having already been successfully built for the new release branch
- QA blocked from testing, sometimes for days, while waiting for builds to succeed
- job configuration fanned out many layers deep across both the devops-jenkins repo and the Jenkins UI

## the fix

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

*CI/CD pipelines that nobody understands are a reliability risk. If your build system has grown beyond anyone's ability to reason about it, [we can help untangle it](mailto:hello@whatsdown.dog).*
