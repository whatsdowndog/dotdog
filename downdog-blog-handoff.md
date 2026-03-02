# Downdog Blog Post Handoff

## Goal

Rewrite 7 draft blog posts in `content/blog/` to sound like Spike. Current drafts have the right structure, source material, and Downdog CTAs, but the voice is wrong - too polished, too generic, too "good SRE blog post" rather than distinctively Spike.

## Voice corpus

Read ALL of these before attempting any writing. The goal is to internalise Spike's voice, not just extract rules from it.

### Primary voice samples (Spike's own writing)
- `/tmp/Spike_ 97 things essays.md` - two essays written for a book ("Optimize for MTTBTB" and "If you're doing runbooks, do them well"). Best sample of Spike's natural writing voice for a technical audience.
- `/Users/spike/src/github.com/whatsdowndog/dotdog/content/blog/whats-downdog.md` - first blog post, casual/personal tone
- `/Users/spike/src/github.com/whatsdowndog/dotdog/content/blog/failing-successfully.md` - Spike will likely have edited this by the time you pick it up. Read it to see what they changed.
- `/tmp/email` - client proposal email. More formal but still Spike's voice. Contains engagement models, rates, and detailed bios for both Spike and Isobel. NOTE: contains client name (Propel Holdings) - do not use on the public site.

### Source material for blog content
- `/tmp/Bring your own system design.md` - interview assignment about building a Transactions API at Kingfisher (large European retailer). Source for: data-inconsistency, choose-technology-your-team-can-operate, caching-can-make-you-slower, building-while-teaching.
- `/tmp/DuckDuckGo - Test Project 1 (SL).md` - interview assignment about BFCM preparation at Shopify (not named, referred to as "a major e-commerce platform"). Source for: failing-successfully, reliability-is-a-communication-problem, making-bfcm-boring.
- `/tmp/bfcm.jpeg` - chart showing "Peak CPU across Shopify" with Architecture Tests 1-3 (July-Aug 2021) and Scale Tests 1-4 (Oct 2021) leading up to BFCM 2021
- https://shopify.engineering/scale-performance-testing - public blog post written by Jordan Neufeld (the colleague Spike trained to take over BFCM testing). Useful for public details but don't copy from it.
- https://x.com/ShopifyEng/status/1465806698954772489 - tweet with the BFCM test timeline image

## NDA constraints
- Do NOT name Shopify as the employer. "A major e-commerce platform" is fine. People can figure it out.
- Do NOT name Cloudflare as the edge vendor. Just say "edge vendor."
- Do NOT name the consulting client (Propel Holdings). "A major Canadian fintech" is what's used on the site.
- Kingfisher can probably be referred to as "a large European retailer" - check with Spike.

## What's been done
- 7 draft posts created in `content/blog/` with `draft = true`
- Each has structure, content drawn from source material, and a Downdog CTA at the end
- Openings were revised to be more specific/experiential (but still don't sound like Spike)
- Self-critique identified: posts read as advice for SREs, not as stories that would make an exec pick up the phone

## What needs to happen
- Go paragraph by paragraph with Spike, asking questions to clarify and personalise
- ASK SPIKE QUESTIONS - things that will help add specific detail, correct inaccuracies, or capture their actual perspective. Don't guess.
- Don't fixate on durations/timings as a way to make things feel concrete. Focus on what happened and what was learned.
- The posts need to sound like the book essays and the blog post, not like a well-written SRE blog by a stranger
- Spike should probably write or heavily edit the first post, then use that as the voice calibration for the rest

## Voice notes (from previous analysis, but these rules alone are NOT enough)
- Opens with a vivid scene or provocative statement
- Conversational but every sentence earns its place
- Acknowledges tradeoffs and downsides honestly
- Dashes (" - ") not em-dashes
- British spelling (organisations, recognising)
- Lowercase headings
- Bold for key principles
- Physical-world analogies
- Ends with a memorable line
- Practical - always "so what do you actually do?"

## Other site changes made in this thread
- `content/services/_index.md` - removed "not seeking clients", added "how we work" section with engagement models, bolded company names in track record, fixed typos
- `content/_index.md` - removed ESME/product section
- Blog post dates staggered across Jan-Feb 2026 on varied weekdays

## The 7 draft posts
1. `failing-successfully.md` (Jan 14) - running performance tests in production, BFCM source
2. `data-inconsistency.md` (Jan 22) - legacy system data problems, Kingfisher source
3. `reliability-is-a-communication-problem.md` (Jan 28) - cross-org communication, BFCM source
4. `choose-technology-your-team-can-operate.md` (Feb 5) - rejecting ElasticSearch, Kingfisher source
5. `caching-can-make-you-slower.md` (Feb 9) - Elasticache slower than Postgres, Kingfisher source
6. `building-while-teaching.md` (Feb 19) - enablement as delivery, Kingfisher source
7. `making-bfcm-boring.md` (Feb 26) - making heroic efforts repeatable, BFCM source
