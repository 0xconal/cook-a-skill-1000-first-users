# first-1000-users

| Field | Details |
|-------|---------|
| **Skill Name** | first-1000-users |
| **What it automates** | Takes a product spec and returns a complete community seeding playbook: which communities to target, what signals to search for, and what to write — customized per platform |
| **BEFORE: manual** | Founder spends 2–3 hours/day browsing Reddit and Slack, guessing which subreddits matter, writing replies from scratch, no idea if the tone is right for each platform — 3+ hours per outreach cycle |
| **AFTER: with skill** | Paste product spec → get community map, 20+ search signals, reply templates, and DM templates across 4 platforms — ready in under 10 minutes |
| **Tools / AI used** | Claude (Project or Custom GPT), no API keys, no scripts, no installation |
| **Limitation** | Does not execute outreach — you run searches and send messages manually. Community data may be outdated (AI knowledge cutoff). Templates need personalization before sending. |
| **Roadmap** | Auto-search execution via browser automation; real-time community activity scoring; Slack workspace discovery; template A/B tracking |

---

## Outputs

Drop in a product spec. Get back:

1. **Community Map** — Reddit (5–8 subreddits) + Slack (3–5 workspaces) + X + Hacker News, ranked by relevance with entry strategy and DM culture per community
2. **Search Signal Library** — 20+ phrases across 5 categories (Direct Request → Discussion), each with per-platform search queries and Reply / DM / Both guidance
3. **Reply Templates** — 3 variants per signal type per platform (experience-based, comparison-based, problem-solving)
4. **DM Templates** — Reddit, Slack, X — reference the person's post, lead with value, close without pressure
5. **Platform Tone Guide** — voice, product mention position, and red flags for each platform

---

## How to Use

1. Open Claude → create a new **Project**
2. Paste `skill/SKILL-v4.md` into the Project's **Custom Instructions**
3. Start a new chat in that Project
4. Paste your product spec and ask: *"Generate my community seeding playbook"*

Works on Custom GPT too — paste into System Prompt.

---

*The AI drafts. You decide what gets sent.*
