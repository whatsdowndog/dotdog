+++
title = "Building While Teaching: The Enablement Problem"
date = 2026-02-19
draft = true
+++

**TL;DR:** When the team inheriting your system is also the team you're building it with, simplicity stops being a preference and becomes a hard constraint.

## the constraint

Two months to go live. Twelve months to walk away entirely. Four of us from the consultancy - tech lead, two engineers, a business analyst - embedded inside a large European retailer, sitting across from five of their people. After a year, whatever we'd built would be theirs. Not "supported by us with a retainer." Theirs.

This wasn't a "build it and throw it over the wall" engagement. The consultancy contract included what we called the enablement piece - pairing, TDD, knowledge transfer. The client's engineers weren't just stakeholders. They were the future owners. Every decision we made had to survive our departure.

That single constraint - **the team inheriting the system is the team you're building it with** - shaped every technical choice we made for the next year.

## pairing as delivery

Here's the thing most organisations get wrong about pairing: they treat it as a nice practice, something you do when you have spare capacity. On this project, pairing wasn't a practice. It was a deliverable.

When your client's engineers are sitting next to you writing code, they're not just learning your patterns - they're pressure-testing your decisions in real time. Can this person reason about the code we're writing together? Do they understand why we're writing it this way? If the answer is no, you have two options: teach them, or simplify the design until the answer is yes.

**Pairing made us slower in week one and faster by month three.** The client engineers built confidence. They started pushing back on decisions, asking better questions, catching mistakes. By the time we left, they weren't maintaining a system someone else built. They were maintaining a system they helped build. That distinction matters enormously.

## what "simple" actually requires

We needed a search capability across transaction data. ElasticSearch was - on paper - the better technical fit. Better querying, better full-text search, purpose-built for the problem. We rejected it.

Why? Because there was no managed offering available to the client, and their team had never operated an ElasticSearch cluster. We could have built it, documented it, written runbooks for it. But operating a distributed search engine is not something you pick up from a runbook. It's something you learn by getting paged at 3am for six months.

So we used Postgres. We bypassed the ORM entirely and wrote SQL by hand. The number of queries was small enough that full control over performance was worth more than the convenience of an abstraction layer. Every query was readable. Every query was debuggable by someone who understood SQL but had never seen our codebase.

**Simple is not easy.** Simple means doing the hard work upfront - thinking through the problem, resisting the temptation to reach for a powerful tool when a basic one will do, writing more code yourself instead of leaning on a framework that hides complexity you'll eventually need to understand. Simple requires discipline. Complexity is the path of least resistance.

## documentation as a first-class deliverable

On most projects, documentation is the thing you promise to write and never do. On this project, it was a contractual obligation - and rightly so.

We treated runbooks, READMEs, design documents, and architectural decision records with the same rigour as production code. They were reviewed, tested against reality, and kept up to date. An ADR that explained why we rejected ElasticSearch was worth as much as the code that replaced it, because six months after we left, someone would ask "why didn't they just use Elastic?" and the answer needed to be there.

**If the people reading your documentation aren't the people who wrote the code, the documentation has to be good enough to stand on its own.** Not "good enough for us" - good enough for someone who has never paired with you, never heard you explain the tradeoff in a standup, never seen the Slack thread where the decision was made.

## the feedback loop

Building while doing enablement is hard. It's slower. It requires patience when you'd rather just write the code yourself and move on. But it creates a feedback loop that makes the system better.

If you can't explain a decision to the person pairing with you, the decision might be wrong. Not always - sometimes it's genuinely complex and the right call is to teach. But more often than you'd expect, the difficulty of explanation is a signal that the design is too clever, the abstraction too leaky, the technology too exotic.

The client engineers were an ongoing usability test for our architecture. Every confused look was a code smell. Every "why don't we just..." was worth taking seriously. We didn't always change course, but we always had to articulate why.

When we walked away, the system was simple, well-documented, and understood by the people running it. That's not a compromise. That's the whole point.

*Enablement is built into [how we work](/services). We don't build things your team can't own after we leave. If that sounds like what you need, [get in touch](mailto:hello@whatsdown.dog).*
