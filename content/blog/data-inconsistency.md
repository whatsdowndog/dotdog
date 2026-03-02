+++
title = "Data Inconsistency is the Real Legacy System Problem"
date = 2026-01-22
draft = true
+++

> **tl;dr:** When you're building modern APIs on top of legacy systems, the hardest problem isn't the old tech stack - it's that the data is wrong in ways nobody fully understands.

## building on top of something nobody understands

- three months in, the message queue finally came online. first thing it sent us: gift cards
- entirely new data type. nobody mentioned it during months of requirements gathering
- not because anyone was unhelpful - the system was so old that nobody knew gift cards flowed through that queue. knowledge lost to reorgs and turnover
- context: large European retailer, 1,300 stores, 9 countries, >£1.3bn annual sales
- entire platform on Oracle ATG, extended over years. data via XML, CSV dumps to FTP, IBM WebSphere queues
- nobody - not even the client - had a complete picture
- we were brought in to build modern APIs on top ("back-end for front-end"). product, prices, stock, then transactions
- our stack: Spring Boot, Java, Postgres, AWS. clean on our side. all the interesting problems were on the boundary

## the zero-downtime data loading problem

- legacy system exported daily CSV dumps over FTP. that's what we had until queues were ready (no ETA)
- constraint: load fresh data daily with zero downtime or latency spike
- solution: COPY into temp tables, run consistency validations, rename to replace live tables, drop old ones. near-instant swap from API's perspective
- not clever, not novel. works because there's nothing to go wrong
- boring is a feature when you're building for handover

## continuous validation, not just schema checks

- legacy data: schema can be perfectly valid while data is completely wrong
- prices that don't add up, negative stock counts, rows referencing products that don't exist in the product feed
- not schema problems - accumulated result of years of ad-hoc integrations, manual fixes, undocumented business rules
- built continuous validation into the loading pipeline. every import: check totals reconcile, relationships hold, numbers make sense
- when rows didn't add up (frequently): save them anyway but flag them, generate report, send back to upstream teams
- **validate continuously, not once.** data quality in legacy systems keeps happening - upstream changes in ways nobody announces
- validation pipeline was also a communication channel back to the teams who owned the data

## surprises at go-live

- gift cards were the most dramatic example, but every new integration point surfaced something nobody anticipated
- not because anyone was withholding info - the system had been modified so many times that no single person had a complete map
- **assume there are things nobody has told you about, because nobody remembers they exist**

## designing for handover

- two months to go-live, twelve months to full handover. client engineers own it after we leave
- shaped every technical decision: Postgres over ElasticSearch (client could operate it), bypassed ORM for hand-written SQL (small number of queries, full control, easier to debug at 2am)
- paired with client engineers constantly - if they couldn't understand a decision while sitting next to us, wrong decision
- **simplicity isn't a preference when building for handover - it's a hard constraint**

## the real risk isn't performance

- people talk about legacy system risks as: performance, availability, outdated tech. those are visible problems
- the bigger risk: **you can't trust the data**
- fastest API in the world is useless if the data feeding it is wrong in ways you can't detect
- continuous verification - ongoing, automated reconciliation - is the only way to build confidence
- legacy systems are like old houses. structure might be sound, but there's always something behind the walls the previous owners forgot to mention

*Building on top of legacy systems without getting buried by them is [something we help with](/services). If your data integration keeps surprising you, [get in touch](mailto:hello@whatsdown.dog).*
