+++
title = "Data Inconsistency is the Real Legacy System Problem"
date = 2026-01-22
draft = true
+++

**TL;DR:** When you're building modern APIs on top of legacy systems, the hardest problem isn't the old tech stack - it's that the data is wrong in ways nobody fully understands.

## building on top of something nobody understands

Three months into the project, the message queue finally came online. The first thing it sent us was data about gift cards. Gift cards - an entirely new data type that nobody had mentioned during months of requirements gathering, architecture discussions, and integration planning. Not because anyone was being unhelpful, but because the system was so old and so large that the people we were working with genuinely didn't know gift cards flowed through that particular queue. The knowledge had been lost to team reorganisations and staff turnover.

That's what working with legacy systems is actually like. This was a large European retailer - 1,300 stores across nine countries, well over a billion pounds in annual sales. The entire e-commerce platform ran on Oracle ATG, extended and customised over years by dozens of teams. Data moved around via XML, CSV dumps to FTP servers, and IBM WebSphere message queues. Nobody - not even the client - had a complete picture of how it all fit together.

We were brought in as an external consultancy to build modern, performant APIs on top of this. A "back-end for front-end" layer so that other teams could build new customer experiences without touching the legacy platform directly. Product data first, then prices, then stock, then transactions. Spring Boot, Java, Postgres, AWS. Clean and straightforward on our side. The interesting problems were all on the boundary.

## the zero-downtime data loading problem

The legacy system exported data as daily CSV dumps, sent over FTP. That was what we had to work with until the message queues were ready - and nobody could tell us when that would be.

The constraint was simple: load fresh data every day without any downtime or latency spike to the API. The solution was equally simple. Copy the CSV data into temporary tables. Run consistency validations against those temp tables. If the data passes, rename the temp tables to replace the live ones, then drop the old tables. The whole swap is near-instant from the API's perspective.

It's not clever. It's not novel. It's the kind of thing that works precisely because there's nothing to go wrong. And when you're building a system that has to be handed off to a team that didn't build it, **boring is a feature**.

## continuous validation, not just schema checks

Here's the thing about legacy data - the schema can be perfectly valid while the data is completely wrong. Prices that don't add up. Stock counts that went negative. Rows that reference products that don't exist in the product feed. You can't predict these problems from the schema alone, because they're not schema problems. They're the accumulated result of years of ad-hoc integrations, manual fixes, and business rules that were never documented.

So we built continuous validation into the data loading pipeline. On every import, we'd check that totals reconciled, that relationships held, that the numbers made sense. When rows didn't add up - and they frequently didn't - we'd save them anyway but flag them, then generate a report and send it back to the upstream teams for investigation.

**The critical insight: validate continuously, not once.** Data quality in a legacy system isn't a problem you solve during the initial integration. It's a problem that keeps happening, because the upstream system keeps changing in ways nobody announces. The validation pipeline wasn't just a technical safeguard - it was a communication channel back to the teams who owned the data.

## surprises at go-live

Remember those gift cards from the opening? That was just the most dramatic example. Throughout the project, every new integration point surfaced something nobody had anticipated. Not because anyone was withholding information - because the system had been extended and modified so many times that no single person had a complete map.

If you're integrating with a legacy system, **assume there are things nobody has told you about, because nobody remembers they exist**. Build your system to handle surprises gracefully rather than assuming you have a complete picture. You don't. You never will.

## designing for handover

We had two months to go-live and twelve months to full handover. The client's engineers would own this system after we left, and they needed to be able to run it, debug it, and extend it without calling us.

That constraint shaped every technical decision we made. We chose Postgres over ElasticSearch because the client team could operate it. We bypassed the ORM and wrote SQL by hand because the number of queries was small enough that full control was worth more than convenience - and hand-written SQL is easier to reason about when you're hunting a production issue at 2am. We paired with the client engineers constantly, not because it was a nice thing to do, but because if they couldn't understand a decision while we were sitting next to them, it was the wrong decision.

**Simplicity isn't a preference when you're building for handover - it's a hard constraint.** Every clever abstraction, every "elegant" solution, every technology choice that requires specialist knowledge is a liability. The system doesn't need to impress anyone. It needs to be running and maintainable a year after you've gone.

## the real risk isn't performance

When people talk about the risks of legacy systems, they usually mean performance, availability, or outdated technology. Those are real problems, but they're problems you can see. The bigger risk is subtler: **you can't trust the data**.

You can build the fastest API in the world, but if the data feeding it is wrong in ways you can't detect, you've just made bad data available faster. Continuous verification - not one-off checks, not schema validation, but ongoing, automated reconciliation of the data flowing through your system - is the only way to build confidence that what you're serving is actually correct.

Legacy systems are like old houses. The structure might be sound, but there's always something behind the walls that the previous owners forgot to mention. The smoke alarm isn't optional.

*Building on top of legacy systems without getting buried by them is [something we help with](/services). If your data integration keeps surprising you, [get in touch](mailto:hello@whatsdown.dog).*
