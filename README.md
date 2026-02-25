# first-1000-users

Drop in your product spec. Get a complete playbook: which communities to target, what signals to search for, and platform-specific messages ready to send.

---

## Why this exists

Most solo founders know they need to do distribution. They just don't do it — not because they're lazy, but because the first step is unclear and every wrong move (wrong subreddit, wrong tone, wrong signal) wastes time and kills momentum.

This skill removes the guesswork. It sharpens your ICP before generating anything, tells you exactly where your users are already talking about your problem, gives you the right search queries for each platform, and writes replies in the tone that each community actually responds to.

**The AI handles the research and drafting. You handle the sending.**

No automation. No fake engagement. Just a faster path from "I have a product" to "I have real users."

---

## Quick Start

1. Open `skill.md`
2. Paste the contents into your Claude Project Instructions (or Custom GPT System Prompt)
3. Drop in your product spec — get your playbook

No installation. No API keys. No dependencies.

---

## File Structure

```
first-1000-users/
│
├── README.md                    ← You are here
├── skill.md                     ← Runtime prompt — paste this into Claude/GPT ✅
├── spec.md                      ← Full specification: all 3 modes ✅
├── skill-card.md                ← Overview: what the skill does, how it works
│
├── ai showcase/                 ← AI process screenshots + demo files
│
├── CHANGELOG.md                 ← Version history + reasoning
├── CLAUDE.md                    ← Design decisions (why the skill is built this way)
├── processing-logic-v5.1.md     ← How the AI processes inputs across all 3 modes
└── vibefounderresearch.pdf      ← GTM research: solo founders × vibe coding
```

---

## Which File to Use

| If you want to... | Use this |
|-------------------|----------|
| Run the skill today | `skill.md` |
| Understand what the skill does | `skill-card.md` |
| Read the full specification | `spec.md` |
| Understand the AI's processing flow | `processing-logic-v5.1.md` |
| See the research behind the strategy | `vibefounderresearch.pdf` |
| See AI process during development | `ai showcase/` |

---

## Version History

| Version | What changed |
|---------|-------------|
| **v5.1** | Check-in Mode — third mode that closes the feedback loop after 20 interactions |
| **v5.0** | ICP Sharpening. Output 0 (Start Here). Competitor-aware reply variant. Live Post Mode. Ongoing Cadence. |
| v4.0 | Removed execution. Added X and Hacker News. Pure AI research + draft. |
| v3.2 | Reddit-only with Playwright browser automation |
| v3.1 | Reddit-only with Reddit API via OpenClaw |
| v2.3 | Original prompt-only concept, Reddit + Slack |

---

## Roadmap — Solving Execution

The current skill solves **research and drafting**. The next problem is **execution**.

Right now, the founder still has to:
- Run the search queries manually
- Find the relevant threads
- Personalize and send each message themselves

That's the right constraint for v5 — it keeps outreach authentic. But it's also the bottleneck that causes founders to stop after week 1.

**The next version aims to close this gap:**

| Problem | Solution |
|---------|----------|
| Running searches manually every day | Browser automation that runs signal queries and surfaces matching threads — founder reviews, not AI decides |
| Not knowing if a thread is worth engaging | Real-time relevance scoring based on signal type, recency, and community activity |
| Losing track of what's been sent | Lightweight outreach tracker — which threads were replied to, which DMs are waiting |
| No Slack workspace discovery | Automated discovery of open workspaces relevant to the ICP |

**The constraint that won't change:** the human still sends every message. Automation handles the *finding*, not the *sending*. The moment the AI posts on the founder's behalf, the authenticity that makes this strategy work disappears.

---

*The AI drafts. You decide what gets sent.*
