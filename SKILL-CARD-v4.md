# 🎯 first-1000-users

> Find people already talking about the problem you solve — and show up with the right message on the right platform.

---

## What This Skill Does

`first-1000-users` is an AI skill that turns a product spec into a complete community seeding playbook. It tells you where your users hang out, what they say when they need something like your product, and exactly what to write — customized per platform.

No scripts. No automation. No API keys. AI does the research and drafting. You do the sending.

---

## How It Works

```
You provide:   Product spec (.md file or pasted text)

AI returns:    1. Community Map       — where to show up
               2. Search Signals      — what to search for
               3. Reply Templates     — what to post publicly
               4. DM Templates        — what to send directly
               5. Platform Tone Guide — how to write for each platform
```

---

## Platforms Covered

| Platform | What's mapped |
|----------|--------------|
| **Reddit** | 5–8 subreddits with rules, DM culture, entry strategy |
| **Slack** | 3–5 public workspaces with best channels |
| **X (Twitter)** | 5–8 accounts, hashtags, and search patterns |
| **Hacker News** | Ask HN patterns, Show HN strategy, comment opportunities |

---

## Input

A `.md` file (or pasted text) with:

| Field | Example |
|-------|---------|
| Product name & one-liner | "first-1000-users – AI skill that maps where your users hang out and drafts what to say to them" |
| Problem it solves | "Founders waste hours browsing Reddit and Slack with no idea which communities matter or what to write" |
| Who it's for | "Solo founders and indie hackers pre-revenue, no marketing budget" |
| Key features | Multi-platform community map, search signals, reply + DM templates per platform |
| Pricing | Free |
| Stage | Beta |
| URL | not yet |
| Competitors | Generic ChatGPT prompts, growth agencies, manual Reddit browsing |

---

## Output Details

### 1. Community Map
Per-platform list of communities ranked by relevance. Each entry includes why it's relevant, self-promo rules, DM culture, and a specific entry strategy.

### 2. Search Signals
20+ signals across 5 categories — from explicit tool requests to personal frustration. Each signal includes per-platform search queries and a clear Reply / DM / Both recommendation.

### 3. Reply Templates
3 variants per signal type per platform (experience-based, comparison-based, problem-solving). Every reply adds genuine value before mentioning the product.

### 4. DM Templates
Platform-specific DMs (Reddit, Slack, X) that reference the person's actual post, offer a real insight first, and close without pressure. HN is excluded — not appropriate there.

### 5. Platform Tone Guide
A quick-reference cheat sheet: voice, opening move, product mention position, and red flags for each platform.

---

## Platform Tone Summary

| Platform | Voice | Product mention |
|----------|-------|----------------|
| Reddit | Peer who's been through it | Last sentence, casual |
| Slack | Helpful colleague | After adding value |
| X | Opinionated practitioner | Only if directly relevant |
| Hacker News | Technical peer | Last line only, or skip entirely |

---

## What the AI Does vs What You Do

| AI | You |
|----|-----|
| Read spec and extract variables | Provide an honest, specific product spec |
| Map communities across 4 platforms | Verify communities are still active |
| Generate search signals with search queries | Run the searches manually |
| Draft reply and DM templates | Personalize drafts before sending |
| Build platform tone guide | Use it for ongoing outreach |
| | Send everything manually |

---

## Runs On

| Platform | Notes |
|----------|-------|
| **Claude Project** | Paste SKILL.md into Project Instructions |
| **Custom GPT** | Paste SKILL.md into System Prompt |
| **Claude Code** | Use SKILL.md as skill file |
| **OpenClaw** | Use SKILL.md as skill file |

No installation. No API keys. No dependencies.

---

## Success Criteria

| Criteria | Target |
|----------|--------|
| Platform coverage | All 4 platforms mapped with specific communities |
| Signal quality | Every signal is product-specific, no generic placeholders |
| Template tone | Platform tones clearly distinct from each other |
| DM compliance | Every DM references a specific post, no HN DMs |
| Human test | Every template passes "would you post this on your real account" |

---

## Version

| | |
|--|--|
| **Version** | 4.0.0 |
| **Updated** | February 2026 |
| **Key change from v3.x** | Removed all execution (Playwright, Python scripts, API, Telegram). Added X and Hacker News. Pure AI research + draft skill. |

---

## Files

```
first-1000-users/
├── SKILL.md               # System prompt — paste into Claude Project or Custom GPT
├── SKILL-CARD.md          # This file — overview
├── spec-v4.md             # Full specification
└── processing-logic-v4.md # How the AI processes a spec into outputs
```

---

*The AI drafts. You decide what gets sent.*
