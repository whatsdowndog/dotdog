+++
title = "All Good Things (Come To An End)"
date = 2026-02-19
draft = true
+++

"Living this way I stress less" -- _Nelly Furtado, software consultant_

> **tl;dr** Creating sustainable, durable change is hard - even more so when you're not around to steward it after the engagement ends. Focus on long-term enablement through pairing, documentation, and disciplined simplicity.

## context, constraints

- two months to go live. twelve months to walk away entirely
- four of us from the consultancy (tech lead, two engineers, BA) embedded inside a large European retailer, sitting across from five of their people
- after a year, whatever we'd built would be theirs. not "supported by us with a retainer." theirs
- contract included the enablement piece - pairing, TDD, knowledge transfer
- client engineers weren't just stakeholders. they were the future owners
- **the team inheriting the system is the team you're building it with** - this shaped every technical choice

## pairing as delivery

- most organisations treat pairing as a nice practice, something you do with spare capacity
- on this project: pairing wasn't a practice, it was a deliverable
- client engineers sitting next to you writing code = pressure-testing your decisions in real time
- can they reason about this code? do they understand why we're writing it this way?
- if no: teach them, or simplify the design until the answer is yes
- **pairing made us slower in week one and faster by month three**
- client engineers built confidence, started pushing back, asking better questions, catching mistakes
- by the time we left: they weren't maintaining a system someone else built. they were maintaining a system they helped build

## what "simple" actually requires

- needed search across transaction data. ElasticSearch was the better technical fit on paper
- rejected it: no managed offering, client team had never operated an ES cluster
- could have built it, documented it, written runbooks. but operating a distributed search engine isn't something you pick up from a runbook
- used Postgres. bypassed ORM, wrote SQL by hand. small number of queries, full control, every query readable and debuggable
- **simple is not easy.** doing the hard work upfront, resisting powerful tools when basic ones will do, writing more code instead of leaning on a framework that hides complexity
- simplicity requires discipline. complexity is the path of least resistance

## documentation as a first-class deliverable

- on most projects: documentation is the thing you promise to write and never do
- on this project: contractual obligation, and rightly so
- runbooks, READMEs, design docs, ADRs - treated with same rigour as production code
- an ADR explaining why we rejected ElasticSearch was worth as much as the code that replaced it
- six months after we leave, someone asks "why didn't they just use Elastic?" and the answer needs to be there
- **docs have to be good enough to stand on their own** - for someone who never paired with you, never heard you explain the tradeoff

## build feedback loops

- building while doing enablement is hard. slower. requires patience
- but creates a feedback loop that makes the system better
- if you can't explain a decision to the person pairing with you, it might be the wrong decision
- client engineers were an ongoing usability test for the architecture
- every confused look = code smell. every "why don't we just..." = worth taking seriously
- when we walked away: system was simple, well-documented, understood by the people running it. that's not a compromise. that's the whole point

*Enablement is built into [how we work](/services). We don't build things your team can't own after we leave. If that sounds like what you need, [get in touch](mailto:hello@whatsdown.dog).*
