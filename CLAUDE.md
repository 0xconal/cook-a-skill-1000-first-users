# CLAUDE.md — first-1000-users

**Context reset anchor.** If you're reading this file, you need to quickly understand why this skill is designed the way it is — not *what* it does, but *why*.

Read this file before reading anything else.

---

## What this skill is (1 line)

AI takes a product spec → challenges the ICP → returns a complete playbook for founders to seed into real online communities. No automation. No API. Humans send everything manually.

---

## Non-negotiable design decisions — and the reasoning behind them

### 1. ICP Sharpening must run before everything else (cannot be skipped)

**Problem:** Founders write specs in marketing language. "Target: entrepreneurs." "Problem: streamlined productivity." If the AI takes this input and generates straight away → the entire output will be generic.

**Decision:** Step 0 is a validation gate. The AI must challenge the spec first, wait for the founder to confirm the ICP is correct, then generate.

**Why not let the AI infer:** The AI can guess what "entrepreneurs" means — but the founder needs to *discover for themselves* that their ICP is still vague. That's the real value, not just output quality.

---

### 2. No execution — humans send every message manually

**History:** v3.x built Playwright automation to auto-post on Reddit. It worked. Removed entirely in v4.

**Why it was removed:** Platform seeding works because it *looks real*. Once automation enters, it no longer looks real. Reddit bans bots. Slack workspaces kick spammers. Communities detect patterns. The entire strategy collapses.

**Principle:** "The AI drafts. You decide what gets sent." This is a line that cannot be moved.

---

### 3. Output 0 (Start Here) must come before Outputs 1–6

**Real-world problem:** Founder receives a 7-section playbook → gets overwhelmed → does nothing. A full playbook is information, not action.

**Decision:** Output 0 is a list of 5 specific actions for Week 1, with exact search queries, time estimates, and clear instructions. Founders can start immediately without reading the rest.

**Rule:** Output 0 must be sufficient to execute without reading Outputs 1–6.

---

### 4. 4 platforms (Reddit, Slack, X, HN) — no more

**Intentionally excluded:**
- LinkedIn → too formal, not where users talk about real pain points
- Product Hunt → launch event, not ongoing seeding
- Discord → search mechanism not strong enough for signal detection
- Facebook Groups → declining, hard to access programmatically

Reddit + Slack + X + HN cover ~90% of where indie founders and early adopters actually have conversations. More platforms = more noise, not more signal.

---

### 5. Platform tone rules must be explicit down to specific banned words

**Why not let the AI self-adjust tone:** Because founders don't know they're doing it wrong. A reply with an em dash on Reddit gets downvoted immediately — but the founder thinks good content means tone doesn't matter. Wrong.

**Decision:** Each platform has a banned words list, banned opening phrases, and structural rules. Not guidelines — hard rules.

**Concrete examples:** Reddit bans em dashes (–). Not "avoid" — banned. HN bans "Great question!". X bans hedging language. These rules come from real observation of how communities reject content.

---

### 6. Competitor-aware variant is the 4th variant — not optional

**Problem:** Founders typically avoid mentioning competitors for fear of "advertising for the competition." But users are already comparing alternatives when searching for a solution — if a reply doesn't acknowledge this, it reads as less authentic.

**Decision:** Variant 4 is always present if the COMPETITORS field is non-empty. Fixed structure: acknowledge what the competitor does well → name the specific gap → position the product as the natural next step, not a replacement.

**Banned:** FUD, vague superiority claims ("we're better"), attacking the competitor's core value prop.

---

### 7. Live Post Mode is a separate mode — not a Playbook shortcut

**Problem:** "I found this post, can the AI write me a reply?" is a completely different question from "write me a playbook." If folded into Playbook Mode, the AI would generate a template and adapt it — the output reads like a template.

**Decision:** Live Post Mode is its own pipeline. No templates. Read the actual post, write an actual reply, reuse the exact words of the person who wrote it.

**Quality test:** The person who wrote the post receives this reply/DM — do they think a real person read their post?

---

### 8. Feedback loop after 20 interactions — not 10, not 50

**Why 20:** 10 interactions isn't enough data to distinguish pattern from noise. 50 interactions is 3–5 weeks — too long to pivot if a signal is wrong. 20 interactions = roughly 1–2 weeks of real founder effort.

**What the AI does after check-in:** Reprioritize signals, adjust tone if replies are getting downvoted, narrow the ICP if needed, retire signals that aren't producing results. This is a learning loop — not a one-time generation.

---

## File structure

```
first-1000-users/
├── CLAUDE.md              ← You are here — design decisions
├── CHANGELOG.md           ← Version history + reasoning
├── README.md              ← Quick start
├── SKILL-CARD.md          ← Overview for end users
│
├── skill/
│   ├── SKILL-v5.md        ← Runtime prompt ← PASTE THIS
│   └── SKILL-v4.md        ← Previous version (archived)
│
├── spec/
│   └── spec-v5.md         ← Full specification (source of truth)
│
├── docs/
│   └── processing-logic-v5.md  ← AI processing flow
│
└── web-app/               ← Next.js wrapper (demo + deployment)
```

**File ownership:**
- `SKILL-v5.md` → requires review before changes (runtime behavior)
- `spec-v5.md` → human-controlled, source of truth
- `CLAUDE.md` → update whenever a major design decision is made
- `web-app/` → independent, can iterate quickly

---

## What is NOT in scope (and why)

| Excluded | Reason |
|---------|-------|
| Scheduling / monitoring | Requires infrastructure, outside scope of an AI-only skill |
| Account management | Platform-specific, high ban risk |
| Bulk DM | Unethical, destroys the trust the entire strategy depends on |
| Auto-posting | See point 2 above |
| Real-time community data | AI knowledge cutoff — requires disclaimer |
| LinkedIn outreach | Too formal for early-stage seeding |

---

## Roadmap (documented in SKILL-CARD)

- Auto-search execution via browser automation (when platforms allow)
- Real-time community activity scoring
- Slack workspace discovery
- Template A/B tracking
- Check-in Mode — a third mode to process feedback after 20 interactions

---

*This file is updated whenever a breaking design decision is made. If you disagree with any decision here — that's a conversation to have before changing SKILL.md.*
