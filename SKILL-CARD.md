# first-1000-users

| Field | Details |
|-------|---------|
| **Skill Name** | first-1000-users |
| **Version** | 5.0 |
| **What it does** | Takes a product spec and returns a complete distribution playbook: sharpens your ICP, tells you where to start this week, maps communities across 4 platforms, surfaces buying signals, and generates ready-to-send reply and DM templates — customized per platform |
| **BEFORE: manual** | Founder spends 2–3 hours/day browsing Reddit and Slack, guessing which subreddits matter, writing replies from scratch, no idea if the tone is right for each platform — 3+ hours per outreach cycle, low conversion |
| **AFTER: with skill** | Paste product spec → ICP gets sharpened → Week 1 priorities land immediately → community map, 20+ search signals, 4 reply variants per signal, DM templates, and an ongoing cadence — ready in under 10 minutes |
| **Tools / AI used** | Claude (Project or Custom GPT), no API keys, no scripts, no installation |
| **Limitation** | Does not execute outreach — you run searches and send messages manually. Community data may be outdated (AI knowledge cutoff). Templates need personalization before sending. |
| **Roadmap** | Auto-search execution via browser automation; real-time community activity scoring; Slack workspace discovery; template A/B tracking |

---

## Two Modes

| Mode | When to use | How to trigger |
|------|------------|----------------|
| **Welcome** | No input yet | Say "hi" or "start" |
| **Playbook Mode** | First run, new product | Paste your product spec |
| **Live Post Mode** | Found a real post and want a custom reply | Paste the post directly |

---

## Outputs (Playbook Mode)

Drop in a product spec. Get back:

0. **Start Here** — Prioritized Week 1 action list: which platforms, which searches, what to do when you find a match. Calibrated to your stage and audience. Start here, not at the community map.
1. **Community Map** — Reddit (5–8 subreddits) + Slack (3–5 workspaces) + X + Hacker News, ranked by relevance with entry strategy and DM culture per community
2. **Search Signal Library** — 20+ phrases across 5 categories (Direct Request → Discussion), each with per-platform search URLs and Reply / DM / Both guidance
3. **Reply Templates** — 4 variants per signal type per platform: experience-based, comparison-based, problem-solving, and competitor-aware
4. **DM Templates** — Reddit, Slack, X — reference the person's post, lead with value, close without pressure
5. **Platform Tone Guide** — voice, product mention position, and red flags for each platform
6. **Ongoing Cadence** — Weekly rhythm for 30 min/day or 2–3 hours/week, plus a feedback loop that adjusts the strategy after 20 interactions

---

## Output (Live Post Mode)

Paste a real post or thread. Get back:

- One custom reply written for that specific post (not a template)
- One custom DM written for that specific person (not a template)
- Platform-appropriate tone, their actual words used as reference

---

## What's New in v5

| Addition | Why it matters |
|----------|---------------|
| **Guided onboarding** | Agent greets new users, explains what to provide, and shows a ready-to-fill input template — no guessing where to start. |
| **ICP Sharpening** | Challenges vague audience descriptions before generating anything. Better ICP = better signals = better templates. |
| **Output 0: Start Here** | Founders don't fail from lacking a playbook — they fail from not knowing where to start. This gives them 5 specific actions for Week 1. |
| **Competitor-aware reply variant** | The 4th template variant uses your competitor knowledge to position naturally against what people are already using. |
| **Live Post Mode** | Jump from "found a relevant post" to "have a custom reply ready" in one step — no templates, written for that specific person. |
| **Ongoing Cadence** | Distribution is a habit. The cadence output gives a realistic weekly rhythm and a feedback loop after 20 interactions. |

---

## How to Use

1. Open Claude → create a new **Project**
2. Paste `SKILL-v5.md` into the Project's **Custom Instructions**
3. Start a new chat in that Project
4. Say "hi" — the skill will guide you from there

Works on Custom GPT too — paste into System Prompt.

---

*The AI drafts. You decide what gets sent.*
