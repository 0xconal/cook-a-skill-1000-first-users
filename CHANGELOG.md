# CHANGELOG — first-1000-users

Each version is not just "added features" — each version is a response to something learned from reality. This file documents both *what changed* and *why*.

---

## v5.0 — February 2026

**Theme: From playbook generator → thinking partner**

### Added

**ICP Sharpening (Step 0)**
A mandatory validation step before generating any output. The AI must challenge the spec — "entrepreneurs", "developers", or any audience description not specific enough to find on a single subreddit is rejected. The founder must confirm the Refined ICP before proceeding.

*Why:* v4 output was good when input was good, but when input was vague the output was also vague — and the founder couldn't tell. ICP Sharpening surfaces this problem upfront instead of hiding it inside the output.

**Output 0: Start Here**
5–7 specific actions for Week 1, with exact search queries, time estimates, and clear instructions. Generated immediately after ICP confirmation, before Outputs 1–6.

*Why:* Founders who receive a full playbook don't know where to start. Output 0 solves this — it's executable immediately without reading the rest.

**Competitor-aware reply variant**
The 4th variant within each signal type. Fixed structure: acknowledge the competitor → name the specific gap → position the product as the natural next step. Only generated when the COMPETITORS field is non-empty.

*Why:* Founders tend to avoid mentioning competitors, but users are already comparing alternatives when they search. A reply that doesn't acknowledge this reads as less authentic.

**Live Post Mode**
The second mode. Paste a real post → get a custom reply and a custom DM written specifically for that post. No templates. Quality test: the person who wrote the post must think this came from a real human who actually read it.

*Why:* "I found this post, write me a reply" is a completely different use case from "build me a playbook." Folding it into Playbook Mode would still produce template-like output.

**Output 6: Ongoing Cadence**
Weekly rhythm for 30 min/day and 2–3 hours/week. Feedback loop triggers after 20 interactions — AI reprioritizes signals, adjusts tone, narrows ICP if needed.

*Why:* Distribution is a habit, not an event. A playbook without a cadence means founders do it for one week then stop.

---

## v4.0 — 2025

**Theme: The biggest pivot in the project's history — dropped execution, added platforms**

### Removed

**Playwright automation and Reddit API**
The entire execution layer was removed. Every script for auto-posting, auto-searching, auto-commenting — deleted.

*Why:* Automation makes output look like spam. Communities detect patterns faster than expected. Reddit shadow-bans accounts. Slack workspaces kick members. Platform seeding works because it *looks real* — automation destroys that. "The AI drafts. You decide what gets sent" became the core principle.

### Added

**X (Twitter) and Hacker News**
Two new platforms with distinct tone rules. HN is the most demanding — reply only, no DMs, no marketing language, genuine technical depth required. X is ultra-concise, opinionated, one idea per reply.

*Why:* Reddit and Slack alone weren't sufficient to reach technical founders and indie hackers. HN and X have higher audience density for this segment.

**Clearer signal classification**
5 categories with explicit priority: Direct Request > Comparison > Pain Point > Workflow Question > Discussion. DM only for the first 3 categories. Reply for all except HN.

---

## v3.2 — 2024

**Theme: Browser automation**

### Added

**Playwright browser automation**
Automatically opens Chrome, navigates Reddit, finds posts matching signals, extracts content. No API key required — uses the browser directly.

*Why:* Reddit API was rate-limited and required OAuth. Browser automation bypassed this.

*Outcome:* Functional but fragile. Selectors broke every time Reddit updated its UI. High maintenance cost. → Replaced by a fully manual approach in v4.

---

## v3.1 — 2024

**Theme: Reddit API integration**

### Added

**Reddit API via OpenClaw**
Direct integration with the Reddit API to search and extract posts. Requires OAuth credentials.

*Why:* Automate the search step to save founder time.

*Outcome:* Severe rate limits. Reddit changed API policy. Many founders couldn't complete the OAuth setup. → Led to v3.2 with browser automation.

---

## v2.3 — 2024

**Theme: Adding structure to the prompt-only approach**

### Added

- Buying signal library — specific categories and patterns
- DM templates — dedicated structure for direct messages
- Tone rules — first appearance, covering Reddit and Slack only at this stage

*Why:* v1 and v2 generated output but quality wasn't stable — output quality depended heavily on how the founder phrased their prompt. v2.3 introduced structure to make output consistent.

---

## v1.0 — 2024

**Theme: Proof of concept**

Reddit and Slack only. No template structure, no tone rules, no signal library. Founder pastes spec → AI generates a list of subreddits and some example replies.

*What we learned:* Output was too generic. No way to know which communities were actually relevant. Tone wasn't calibrated to each platform. No guidance on when to reply vs DM. → Drove the entire roadmap from v2 onwards.

---

## Things tried and abandoned

| Tried | Why abandoned |
|-------|--------------|
| Playwright automation (v3.2) | Fragile, high maintenance, looks like a bot |
| Reddit API (v3.1) | Rate limits, OAuth friction, policy changes |
| LinkedIn outreach | Audience mismatch — too formal for seeding |
| Discord integration | Search mechanism too weak for signal detection |
| Auto-reply bot | Destroys trust that the entire strategy depends on |
| Email outreach | Out of scope — requires its own domain and tooling |

---

*If you want to bring back anything from the "tried and abandoned" list — read CLAUDE.md first.*
