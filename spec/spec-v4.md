# SPEC DOCUMENT — `first-1000-users`

**A Prompt-Based AI Skill for Multi-Platform Community Seeding**

Version: 4.0 | February 2026
Status: Active
Platforms: Claude Project, Custom GPT, Claude Code, OpenClaw

**Change from v3.x:** Removed all execution components (Playwright, Python scripts, Reddit API, Telegram bot, rate limiters). Skill is now purely AI-driven: read spec → understand product → map communities → draft content. Added X (Twitter) and Hacker News alongside Reddit and Slack.

---

## 1. Overview

`first-1000-users` is an AI skill that helps founders seed their product into real online conversations. You provide a product spec. The AI returns a complete playbook: where to show up, what signals to look for, and exactly what to say — customized per platform.

No scripts. No automation. No API keys. Just AI + human execution.

### 1.1 Core Idea

Find people who are already talking about the problem you solve. Show up with a helpful reply. If someone is expressing a personal pain point, send them a direct message. The AI handles research and drafting. The human handles sending.

### 1.2 What This Skill Does

- ✅ Reads a product spec and extracts the right variables
- ✅ Maps communities across Reddit, Slack, X, and Hacker News
- ✅ Generates buying signals with per-platform search queries
- ✅ Drafts reply and DM templates customized per platform tone
- ✅ Provides a platform tone guide for ongoing outreach

### 1.3 What This Skill Does NOT Do

- ❌ No execution — the human sends every message manually
- ❌ No browser automation or API calls
- ❌ No scheduling or monitoring
- ❌ No account management

### 1.4 Execution Model

```
AI does:    Read spec → Map communities → Generate signals → Draft templates
Human does: Choose where to engage → Personalize drafts → Send manually → Handle replies
```

---

## 2. Input

A `.md` file (or pasted text) describing the product:

| Field | What to include |
|-------|----------------|
| Product name & one-liner | What it is in one sentence |
| Problem it solves | Pain point in user language, not marketing copy |
| Who it's for | Specific role + context (not "entrepreneurs") |
| Key features | Top 3–5 differentiators |
| Pricing | Free / freemium / paid / open-source |
| Current stage | Pre-launch / beta / live |
| Product URL | Link or "not yet" |
| Competitors | What users do today without the product |

If any field is missing or vague, the AI asks before proceeding.

---

## 3. Output

5 outputs returned in order:

| # | Output | Description |
|---|--------|-------------|
| 1 | **Community Map** | Where target users hang out on all 4 platforms |
| 2 | **Search Signals** | What they say when they need this product |
| 3 | **Reply Templates** | What to post publicly, per platform, per signal type |
| 4 | **DM Templates** | What to send directly, per platform |
| 5 | **Platform Tone Guide** | Quick-reference cheat sheet for ongoing outreach |

---

## 4. Community Map

### 4.1 Reddit

Target: 5–8 subreddits ranked by relevance.

Per subreddit:
- Name + link
- Estimated subscriber count
- Relevance score: HIGH / MEDIUM / LOW
- Why relevant (1 sentence tied to target audience)
- Best thread types to engage with
- Self-promotion rules
- DM culture (accepted / neutral / frowned upon)
- Entry strategy (specific, not generic)

Selection criteria: subreddits where the product's pain phrases are actively discussed, not just where the product category is popular.

### 4.2 Slack

Target: 3–5 workspaces. Only public workspaces (open join). Flag invite-only.

Per workspace:
- Name + join link
- Estimated size
- Relevance score
- Why relevant
- Best channels (#help, #tools, #resources, etc.)
- DM culture
- Entry strategy

### 4.3 X (Twitter)

X has no formal community structure. Map accounts + hashtags + search patterns.

Target: 5–8 entries.

Per entry:
- Type: Account / Hashtag / Search query
- Name or handle
- Why relevant
- Best engagement approach (reply / quote tweet / thread)
- Red flags to avoid

Prioritize accounts where pain phrases appear in replies and quote tweets, not just in posts.

### 4.4 Hacker News

HN has no subreddits or workspaces. Map post types and search patterns.

Provide:
- Ask HN search queries (pain phrases that surface relevant threads)
- Show HN strategy (only if product is beta or live)
- Comment opportunity types
- Audience profile (who on HN matches target audience)
- Tone and approach notes specific to HN culture

---

## 5. Search Signals

5 categories, highest to lowest priority:

| Category | Priority | Engagement |
|----------|----------|-----------|
| Direct Request | Highest | Reply + DM |
| Comparison | High | Reply + DM |
| Pain Point | High | DM first |
| Workflow Question | Medium | Reply only |
| Discussion | Low | Reply only |

Per signal:
- Pattern (phrase someone types)
- Per-platform search queries (Reddit / Slack / X / HN)
- Real example (realistic post as it would appear)
- Engagement channel recommendation
- Strongest platform (where this signal appears most)
- Recency window (how old a post can be and still worth engaging)

Minimum 4 signals per category. All product-specific.

---

## 6. Reply Templates

For each signal category × each platform: 3 variants.

### 6.1 Variant Types

- **Experience-based** — personal story, maker framing
- **Comparison-based** — tried multiple options, here's the breakdown
- **Problem-solving** — methodology first, product mentioned last

### 6.2 Structure (all variants)

1. Acknowledge — understand their specific situation
2. Help — genuine value without requiring the product
3. Bridge — product as one natural option
4. Soft close — offer, not pitch

### 6.3 Platform Tone Rules

**Reddit**
- Lowercase "i", casual, peer-to-peer
- 3–6 sentences, longer if real story
- No em dashes
- Human filler: "honestly", "tbh", "for whatever it's worth"
- Messy numbers: "$6200" not "$6k"
- Product mention: "i built something for this"
- Close: "happy to share if useful"
- Banned words: authentic, leverage, seamless, robust, genuinely, sustainable, valuable
- Banned phrases: "The key insight is", "What worked was [gerund]"

**Slack**
- 2–4 sentences max
- Professional but not corporate
- Product mention: "built something for this"
- Close: "happy to share the link"

**X (Twitter)**
- Fits in a reply or quote tweet
- Opinionated, one strong idea, no hedging
- Product mention: only if directly relevant, positioned last
- Never: reply-guy spam, tagging without reason

**Hacker News**
- Expert peer tone, technical depth
- Product mention: last line only, "I've been working on something related: [link]"
- Banned: "Great question!", marketing language, shallow takes
- Rule: if in doubt, skip the product mention entirely

### 6.4 Quality Test

Read the reply back. Ask:
- Would you post this on your real account right now?
- Is the product mention the first thing you notice? (If yes → move it to the end)
- Could this reply be about any product? (If yes → not specific enough)

---

## 7. DM Templates

For Direct Request, Comparison, and Pain Point signals only.

Never DM from: Discussion threads, HN threads, general Workflow Question threads.
Never DM on X unless the person explicitly invited it.

### 7.1 Structure

1. Reference — specific detail from their actual post (not generic "saw your post about X")
2. Empathize — genuine understanding, no pitch
3. Offer value — one insight or tip before product mention
4. Introduce product — brief, tied to their exact stated problem
5. Low-pressure close — easy to say no

### 7.2 Platform Rules

**Reddit DM**
- 3–4 sentences max
- Opener: reference their specific words, not just the topic
- Close: "happy to share if useful, no worries if not"
- Never open with: "I hope", "I wanted to reach out", "I noticed that", "I came across"

**Slack DM**
- 2–3 sentences max
- Opener: "hey [name], saw your message in #[channel] about [specific thing]..."
- Never cold DM — must reference a specific channel message first

**X DM**
- 2–3 sentences max
- Only if they explicitly invited suggestions
- Drop the link, one sentence close

**HN**
- Do not DM. Not appropriate on HN.

### 7.3 Ethical Guardrails

- ❗ One DM per person — never follow up if no reply
- ❗ Always reference their specific post
- ❗ Respect "no" — thank and move on
- ❗ No bulk or copy-paste DMs — every message personalized
- ❗ Check platform rules before DMing
- ❗ Never DM from HN or X without explicit invite

---

## 8. Platform Tone Guide

A quick-reference cheat sheet for ongoing outreach.

| Platform | Voice | Opening move | Product position | Red flags |
|----------|-------|-------------|-----------------|-----------|
| Reddit | Peer who's been through it | Answer fully first | Last sentence | Link in first comment, reply-to-everyone |
| Slack | Helpful colleague | Answer in 1–2 sentences, offer more | After adding value | Same message across channels |
| X | Opinionated practitioner | One genuine insight | Only if directly relevant | Reply-guy spam, unsolicited DM |
| HN | Technical peer | Substantive standalone comment | Last line only, or skip | Any marketing language, "great question" |

---

## 9. Success Criteria

| Criteria | Target |
|----------|--------|
| Community map coverage | All 4 platforms with specific, relevant communities |
| Signal specificity | Every signal uses product-specific language, no placeholders |
| Template quality | Passes "would you post this on your real account" test |
| Platform differentiation | Reddit / Slack / X / HN tones clearly distinct |
| DM compliance | 100% reference a specific post, 0% from HN threads |

---

## 10. Version History

| Version | Change |
|---------|--------|
| v1.0 | Initial concept — Reddit + Slack only |
| v2.3 | Added buying signal library, DM templates, tone rules |
| v3.x | Added Playwright execution, Reddit browser automation |
| **v4.0** | **Removed all execution. Added X and HN. Pure AI research + draft skill.** |

---

*End of Specification — first-1000-users v4.0*
