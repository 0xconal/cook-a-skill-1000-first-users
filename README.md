Drop in your product spec. Get a complete playbook: which communities to target, what signals to watch for, and what to say to each person.

# first-1000-users

> The AI drafts. You decide what gets sent.

**first-1000-users** is an AI skill that helps founders and marketers seed their product into real online conversations. Drop in a product spec. Get a complete playbook: which communities to target, what signals to watch for, and what to say to each person.

---

## Quick Start

1. Open Claude (claude.ai)
2. Create a new **Project**
3. Copy everything from `skill/SKILL-v4.md` into the Project's **Custom Instructions**
4. Start a new chat in that Project
5. Paste your product spec and ask: **"Generate my community seeding playbook"**

---

## File Structure

```
first-1000-users/
├── README.md               ← Start here
├── SKILL-CARD.md          ← Overview of what the skill does
├── skill/                  ← Paste one of these into Claude or GPT
ↂ     ├── SKILL-v4.md         ← Current version ✅
ↂ    ├── SKILL-v3.2-playwright.md  ₒ Browser-automation only
Jↂ    ├── SKILL-v3.1-reddit-only.md   ← Reddit only, no automation
⊒── spec/                    ← Specification documents
⊒── docs/                   → Processing logic explained
⊒── research/               ← GTM research deck
```

---

## Which File to Use?

| You want to... | Use this file |
|-----------------------------------|----------------------------------|
| Just get the playbook (fastest) | `skill/SKILL-v4.md` |
| Automate browser (OpenClaw) | `skill/SKILL-v3.2-playwright.md` |
| Reddit only, no automation | `skill/SKILL-v3.1-reddit-only.md` |
| Understand the specification | `spec/spec-v4.md` |
| Understand the processing logic | `docs/processing-logic-v4.md` |

---

## Version History

| Version | What changed |
|----------|-----------------|
| v4.0 (current) | Added X/Twitter + Hacker News search; platform-specific outputs; recency windows; multi-thread Reddit strategy |
| v3.2 | Playwright browser automation replaces Reddit API; anti-detection measures |
| v3.1 | Reddit-only focus; prompt-based approach, no code required |
| v2.3 | Reddit API based (now deprecated) |
