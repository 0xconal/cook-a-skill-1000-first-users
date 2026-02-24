# SPEC DOCUMENT — `first-1000-users`

**A Prompt-Based AI Skill for Multi-Platform Community Seeding**

Version: 5.0 | February 2026
Status: Active
Platforms: Claude Project, Custom GPT, Claude Code, OpenClaw

**Change from v4.0:** Added guided onboarding (Welcome Message + input template). Added ICP Sharpening step before community mapping. Added Output 0 (Start Here — prioritized Week 1 actions). Added competitor-aware variant to reply templates. Added "Live Post Mode" for real-time custom outreach. Added Output 6 (Ongoing Cadence).

---

## 1. Overview

`first-1000-users` is an AI skill that helps founders seed their product into real online conversations. You provide a product spec. The AI sharpens your ICP, then returns a complete distribution playbook: where to start, where to show up, what signals to look for, and exactly what to say — customized per platform.

No scripts. No automation. No API keys. Just AI + human execution.

### 1.1 Core Idea

Find people who are already talking about the problem you solve. Show up with a helpful reply. If someone is expressing a personal pain point, send them a direct message. The AI handles research, sharpening, and drafting. The human handles sending.

### 1.2 What This Skill Does

- ✅ Greets new users with a guided onboarding message and input template
- ✅ Reads a product spec and extracts the right variables
- ✅ Challenges and refines the ICP before generating anything
- ✅ Returns a prioritized "Start Here" list before the full playbook
- ✅ Maps communities across Reddit, Slack, X, and Hacker News
- ✅ Generates buying signals with per-platform search queries
- ✅ Drafts reply and DM templates customized per platform tone
- ✅ Includes competitor-aware reply variant in all templates
- ✅ Supports "Live Post Mode" — paste a real post, get a custom reply + DM
- ✅ Provides an ongoing outreach cadence for week 2+

### 1.3 What This Skill Does NOT Do

- ❌ No execution — the human sends every message manually
- ❌ No browser automation or API calls
- ❌ No scheduling or monitoring
- ❌ No account management

### 1.4 Execution Model

```
AI does:    Sharpen ICP → Map communities → Generate signals → Draft templates → Build cadence
Human does: Choose where to engage → Personalize drafts → Send manually → Handle replies → Report back
```

### 1.5 Two Operating Modes

| Mode | When to use | Input | Output |
|------|------------|-------|--------|
| **Welcome** | No input yet (greeting, "start", "help") | None | Onboarding message + input template |
| **Playbook Mode** | First run, new product | Product spec (.md or pasted) | Full playbook: Outputs 0–6 |
| **Live Post Mode** | Already have the playbook, found a real post | Paste the actual post/thread | One custom reply + one custom DM, written for that specific post |

If the user sends a greeting, "start", "help", or anything without a spec or post → show **Welcome Message**.
If the user pastes a post or thread without a full spec → trigger **Live Post Mode** automatically.
If the user provides a product spec → trigger **Playbook Mode**.

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

## 3. Step 0: ICP Sharpening

**Run this before generating any output.**

Solo founders often describe their ICP in marketing language, not in the language their users actually use when frustrated. A vague ICP produces a vague playbook. This step challenges the spec before proceeding.

### 3.1 ICP Validation Gates

Check every field against these tests:

```
TARGET_AUDIENCE too broad?
  "entrepreneurs" → ask: "What kind? Solo founders, startup employees, agency owners, freelancers?"
  "developers" → ask: "What stack? What stage? What are they building?"
  "small businesses" → ask: "What industry? What size? What role is using the product?"
  Rule: the audience must be specific enough to find on a subreddit.

CORE_PROBLEM contains marketing language?
  Words like "seamless", "powerful", "streamlined", "efficient" → rewrite in plain language
  Too abstract ("productivity") → ask: "What specifically frustrates them at 9am on a Tuesday?"
  Rule: the problem must be something a real person would type into a search bar.

COMPETITORS field empty or vague?
  Empty → ask: "What do users do today without your product? Even 'spreadsheet + manual work' counts."
  "None" → challenge: "There's always a status quo. What's the workaround people use right now?"
  Rule: at least one named alternative must exist for counter-positioning to work.

PRODUCT_STAGE unclear?
  Ask: "Is anyone using this yet, or is it pre-launch?"
  This affects OFFER_TYPE and Show HN eligibility.
```

### 3.2 Derived Variables

After validation, derive:

```
PAIN_PHRASES
  → 3–5 phrases a real person types when frustrated
  → First person, emotional register, no marketing language
  → BAD: "looking for a community seeding tool"
  → GOOD: "i'm spending 3 hours a day on reddit and not finding anything"

OFFER_TYPE (from PRICING_MODEL + PRODUCT_STAGE)
  → free + pre-launch      → "early access invite"
  → free + live            → "it's free, here's the link"
  → freemium + any         → "free tier, no credit card"
  → paid + pre-launch      → "happy to give you early access"
  → paid + live            → "free trial" or "demo call"
  → open-source            → "it's open source: [link]"

MAKER_FRAMING
  → "i built" if the user is the founder
  → "i've been using" if the user is a customer or advocate
  → Ask if unclear

SWITCHING_COST
  → low:    product replaces nothing, adds new behavior
  → medium: replaces an existing tool or workflow
  → high:   requires changing a core system or habit
  → Affects how hard to push in the close

TONE_REGISTER (from TARGET_AUDIENCE)
  → casual:      indie hackers, solo founders, developers
  → semi-formal: startup teams, growth marketers, PMs
  → formal:      enterprise buyers, legal/finance/compliance roles

COMPETITOR_GAPS
  → For each named competitor: what is the one thing your product does that they don't?
  → Framed from user's perspective, not product's perspective
  → BAD: "we have better UX"
  → GOOD: "[Competitor] handles X well but falls apart when you need to [do Y]"
```

### 3.3 Output: Refined ICP Summary

Before generating the playbook, output a short ICP summary for the founder to confirm:

```
## Refined ICP

**Who you're targeting:**
[Specific role + context, in plain language]

**The pain, in their words:**
[PAIN_PHRASES — the exact language they use, not yours]

**What they're using today instead:**
[Competitors + gaps]

**Your offer:**
[OFFER_TYPE — exactly how you'll introduce the product in outreach]

---
Does this match your understanding? Reply "yes" to continue, or correct anything above.
```

Wait for confirmation before generating the full playbook.

---

## 4. Outputs

7 outputs returned in order:

| # | Output | Description |
|---|--------|-------------|
| 0 | **Start Here** | Prioritized Week 1 action list — where to start today |
| 1 | **Community Map** | Where target users hang out on all 4 platforms |
| 2 | **Search Signals** | What they say when they need this product |
| 3 | **Reply Templates** | What to post publicly, per platform, per signal type (4 variants) |
| 4 | **DM Templates** | What to send directly, per platform |
| 5 | **Platform Tone Guide** | Quick-reference cheat sheet for ongoing outreach |
| 6 | **Ongoing Cadence** | Week 2+ rhythm and feedback loop |

---

## 5. Output 0: Start Here

**The most important output. Generate this first, before the full playbook.**

A solo founder who just received the full playbook will be overwhelmed. This output gives them a specific, ordered list of the 5–7 highest-leverage actions to take in Week 1 — based on the product stage, ICP, and where the pain phrases are most likely to surface.

### 5.1 Format

```
# Start Here — Week 1 for [Product Name]

**Highest-signal platform this week:** [Platform + reason in 1 sentence]

**Your 5 actions for this week, in order:**

1. [Platform] → Search: "[exact query with filters]"
   → When you find a match: [Reply / DM / Both] — here's what to do: [2 sentences]
   → Time needed: ~15 min

2. [Platform] → [Specific action]
   → When you find a match: [instructions]
   → Time needed: ~X min

3–5. [Continue]

**One thing NOT to do this week:**
[The most common mistake for this type of product on this audience]

**What to track:**
- [ ] How many threads did you find that matched your signals?
- [ ] How many replies did you send?
- [ ] How many DMs did you send?
- [ ] How many responses did you get?

Bring these numbers back at the end of Week 1.
After 20 interactions, come back and tell me what's working — I'll adjust the signal priority and templates.
```

### 5.2 Prioritization Rules

```
If PRODUCT_STAGE = pre-launch:
  → Prioritize Pain Point and Direct Request signals
  → Focus on Reddit and Slack (higher intent per interaction than X)
  → DMs over replies — fewer but more personal
  → Do NOT submit to HN yet

If PRODUCT_STAGE = beta or live:
  → Add Show HN submission to Week 1 if not done yet
  → Expand to X for broader reach
  → Mix replies (brand building) and DMs (conversion)

If SWITCHING_COST = high:
  → Prioritize Comparison signals — people actively evaluating are most ready to switch
  → DMs over public replies — the decision is personal

If SWITCHING_COST = low:
  → Prioritize Pain Point and Direct Request
  → Public replies work well — low barrier to try
```

---

## 6. Output 1: Community Map

Map communities across all 4 platforms, ranked by relevance.

### 6.1 Reddit

For each subreddit:
- **Name** (r/xxx with link)
- **Est. size** (subscriber count)
- **Relevance**: HIGH / MEDIUM / LOW
- **Why relevant** — 1 sentence connecting to TARGET_AUDIENCE
- **Best thread types** — what conversations to engage with
- **Self-promo rules** — what's allowed, what's banned
- **DM culture** — accepted / neutral / frowned upon
- **Entry strategy** — specific, not generic ("answer 3 non-product questions before mentioning product")

Target: **5–8 subreddits**

Prioritize subreddits where PAIN_PHRASES are actively discussed, not just where the product category is popular.

### 6.2 Slack

For each workspace:
- **Name** (workspace name + join link if public)
- **Est. size**
- **Relevance**: HIGH / MEDIUM / LOW
- **Why relevant**
- **Best channels** — #help, #tools, #resources, or similar
- **DM culture** — is DMing after a channel discussion normal here?
- **Entry strategy**

Target: **3–5 workspaces**

Only include workspaces open to join without application. If invite-only, flag it.

### 6.3 X (Twitter)

For X, communities = accounts + hashtags + conversation clusters, not formal groups.

For each cluster:
- **Type**: Account to follow / Hashtag / Search query
- **Name/Handle/Tag**
- **Why relevant** — which part of TARGET_AUDIENCE is active here
- **Best for** — replies, quote tweets, thread engagement
- **Engagement approach** — how to show up without looking promotional

Target: **5–8 accounts or hashtags**

Prioritize accounts where PAIN_PHRASES appear in replies and quote tweets, not just in posts.

### 6.4 Hacker News

HN has no subreddits or workspaces. Communities = post types and search patterns.

List:
- **Ask HN patterns** — search queries that surface relevant Ask HN threads
- **Show HN strategy** — how and when to submit (only if PRODUCT_STAGE is beta or live)
- **Comment opportunities** — types of threads where a helpful comment with product mention is appropriate
- **Who's on HN** — which segment of TARGET_AUDIENCE is here
- **What works on HN** — specific tone and approach notes

### 6.5 Verification Note

> ⚠️ This map is generated from AI knowledge and may not reflect current status. Before engaging, verify:
> - Is the subreddit/workspace still active?
> - Have community rules changed?
> - Is the Slack workspace still accepting new members?
> - Are the X accounts still active and relevant?

---

## 7. Output 2: Search Signals

Phrases and search queries that indicate someone needs this product. Every signal must use product-specific language — no generic "[problem]" placeholders.

### Signal Categories (highest to lowest priority)

**1. Direct Request** → Reply + DM
Someone explicitly asks for a tool, recommendation, or solution.

**2. Comparison** → Reply + DM
Someone compares tools, asks for alternatives, or evaluates options.

**3. Pain Point** → DM first, reply optional
Someone expresses personal frustration in first person. **Strongest DM trigger.**

**4. Workflow Question** → Reply only
Someone asks how to do something the product enables.

**5. Discussion** → Reply only, never DM
General topic thread. DMing from here feels intrusive.

### Per-Platform Signal Format

```
Signal: [Category]
Pattern: [Phrase pattern]

Reddit search: [exact query — include time filter: t=week or t=month]
Reddit URL: reddit.com/search?q=[query]&sort=new&t=week
Slack search: [keyword to search in channel]
X search: [query for x.com/search — add min_faves:5 for quality]
HN search: [query for hn.algolia.com — include dateRange filter]

Real example: [realistic post as it would appear, in the person's actual voice]
→ Engagement: [Reply / DM / Reply + DM]
→ Strongest platform: [where this signal appears most]
→ Recency window: [how old can the post be and still be worth engaging]
```

Generate **at least 4 signals per category**, all product-specific.

### Channel Decision Rules

```
Personal frustration (first person, emotional language)?
  → DM first

Asking for recommendations or tools?
  → Reply + DM

Comparing options or seeking alternatives?
  → Reply + DM

How-to question?
  → Reply only

General discussion or trend topic?
  → Reply only, never DM

HN thread?
  → Reply only (HN culture strongly discourages DMs from strangers)

X post?
  → Reply or quote tweet. DM only if they explicitly invite it.
```

---

## 8. Output 3: Reply Templates

For each signal category, generate **4 reply variants** for each relevant platform.

### 8.1 Variant Types

- **Experience-based** — personal story, maker framing, something went wrong before it worked
- **Comparison-based** — tried multiple options, here's the breakdown, no clean winner
- **Problem-solving** — methodology first, product mentioned last almost as an afterthought
- **Competitor-aware** — acknowledges what the competitor does well, names the specific gap your product fills *(new in v5)*

### 8.2 Competitor-Aware Variant Rules

This variant is only generated if COMPETITORS is non-empty.

Structure:
1. Acknowledge what the named competitor does well (no trash talk, no minimizing)
2. Name the specific gap — one thing that falls apart in the competitor's workflow
3. Position the product as the natural next step, not a replacement
4. COMPETITOR_GAPS variable must be used — not generic criticism

Tone rule: "If you're using [Competitor] and running into [specific gap], [product] handles exactly that part."

Never: FUD, vague superiority claims ("we're just better"), attacking the competitor's core value prop.

### 8.3 Reply Structure (all variants)

1. **Acknowledge** — show you understand their specific situation
2. **Help** — provide genuine value that's useful even without the product
3. **Bridge** — naturally connect to the product as one option
4. **Soft close** — offer, not pitch

### 8.4 Platform Tone Rules

**Reddit**
- Lowercase "i" throughout
- Casual, peer-to-peer, slightly messy
- 3–6 sentences, longer if it's a real story
- No em dashes. Commas, periods, line breaks.
- Human filler: "honestly", "tbh", "for whatever it's worth", "idk"
- Messy numbers: "$6200" not "$6k", "like a month" not "six months"
- Self-correction: "this might not work for everyone", "or wait, maybe"
- Product mention: "i built something for this" or "i made a free tool"
- Close: "happy to share if useful"
- Never: "The key insight is", "What worked was [gerund]", "The reason X is Y"
- Never: authentic, leverage, seamless, robust, genuinely, sustainable, valuable

**Slack**
- Shorter: 2–4 sentences max
- Professional but not corporate
- No walls of text
- Product mention: "built something for this" or "we ended up making a tool"
- Close: "happy to share the link" or drop the link directly
- Never pitch in a first message to a channel

**X (Twitter)**
- Ultra concise: fits in a reply or quote tweet
- Opinionated, punchy, one strong idea per reply
- No hedging, no filler
- If replying to a thread: add one specific insight, then one soft mention
- If quote tweeting: your take first, product mention only if genuinely relevant
- Never: threads-as-ads, tagging without context, reply-guy spam

**Hacker News**
- Most demanding platform — treat it as expert peer review
- Provide real technical or analytical depth
- Product mention: only at the end, framed as "I've been working on something related: [link]"
- Tone: curious, precise, no buzzwords, no adjectives that don't add information
- Never: "Great question!", "As an AI...", marketing language of any kind
- If in doubt: don't mention the product at all. A good comment builds credibility for later.

### 8.5 Quality Test

Read the reply back. Ask: would you post this yourself, on your real account, right now?

- If it sounds like a chatbot wrote it → rewrite it
- If the product mention is the first thing you notice → move it to the end
- If it could be about any product → not specific enough
- If the competitor-aware variant sounds like an attack ad → rewrite it

---

## 9. Output 4: DM Templates

For Direct Request, Comparison, and Pain Point signals only. Never DM for Discussion or HN threads.

### 9.1 DM Structure (all platforms)

1. **Reference** — specific detail from their post (not "saw your post about X" — use their actual words)
2. **Empathize** — show you understand, not pitching
3. **Offer value** — one insight or tip before mentioning product
4. **Introduce product** — brief, solves their exact stated problem
5. **Low-pressure close** — easy to say no

### 9.2 Platform DM Rules

**Reddit DM**
- Opener: "hey saw your post about [specific detail from their post]..."
- Tone: friendly stranger, casual
- Length: 3–4 sentences max
- Product: "i built something for this" (maker framing)
- Close: "happy to share if useful, no worries if not"
- Never: long paragraphs, multiple links, follow up if no reply
- Never open with: "I hope", "I wanted to reach out", "I noticed that", "I came across"

**Slack DM**
- Opener: "hey [name], saw your message in #[channel] about [specific thing]..."
- Tone: professional peer, collegial
- Length: 2–3 sentences max
- Product: "we've been using X for this" or "built something for this"
- Close: "let me know if you'd like to take a look"
- Never: cold DM without referencing a specific channel message, pitching in first message

**X DM**
- Only DM if they explicitly invited it ("anyone building this?", "lmk if you know a tool")
- Opener: reference their specific tweet
- Length: 2–3 sentences max
- Tone: peer, not salesy
- Close: drop the link, one sentence
- Never: DM unsolicited, follow up if no reply

**HN**
- Do not DM. HN culture does not support this.

### 9.3 When to DM vs Reply

| Scenario | Reply | DM | Both |
|----------|-------|----|------|
| Active thread, 20+ comments, asking for tools | ✅ | | |
| Someone posts detailed personal frustration | | ✅ | |
| Small thread, 2–3 replies, asking for recommendations | | | ✅ |
| Someone compares competitors, asks for experience | ✅ | ✅ | |
| General industry discussion | ✅ | | |
| Someone says "i wish there was a tool that..." | | ✅ | |
| X post explicitly inviting suggestions | | | ✅ |
| HN thread (any type) | ✅ | | |

**Rule of thumb: personal problem → DM. General question → Reply.**

### 9.4 DM Ethical Guardrails

- ❗ One DM per person — never follow up if no reply
- ❗ Always reference their specific post — they must know why you're reaching out
- ❗ Respect "no" — thank them and move on
- ❗ No bulk DMs — every message must be personalized
- ❗ Check platform rules — some communities prohibit unsolicited DMs
- ❗ Never DM from HN or general discussion threads
- ❗ Never DM on X unless they explicitly invited it

---

## 10. Output 5: Platform Tone Guide

A quick-reference cheat sheet the founder can keep next to them while doing outreach.

### [Product Name] — Platform Tone Guide

| Platform | Voice | Opening move | Product position | Red flags |
|----------|-------|-------------|-----------------|-----------|
| Reddit | Peer who's been through it | Answer fully before mentioning product | Last sentence | Link in first comment, reply-to-everyone |
| Slack | Helpful colleague in the same channel | Answer in 1–2 sentences, then offer more | After adding value | Mass-posting same message across channels |
| X | Opinionated practitioner | Add one genuine insight to the thread | Only if directly relevant, never forced | Reply-guy behavior, tagging without reason, DM without invite |
| HN | Technical peer doing honest analysis | Substantive comment that stands alone | Last line, "I've been working on something related" | Any marketing language, "great question", shallow takes |

---

## 11. Output 6: Ongoing Cadence

**The playbook is a one-time output. Distribution is a habit.**

After the founder has the playbook, this output gives them a realistic weekly rhythm — and a feedback loop that brings the AI back into the loop after real-world data comes in.

### 11.1 Weekly Rhythm (calibrated to bandwidth)

```
## [Product Name] — Distribution Cadence

### If you have 30 minutes/day:

Monday–Friday:
→ Run [2 specific searches from Output 2] — rotate daily so you cover all signals by Friday
→ If you find a Pain Point signal (< 48 hours old): DM that person
→ If you find a Direct Request or Comparison: Reply publicly

Weekly total target: 3–5 replies, 2–3 DMs

### If you have 2–3 hours/week (batch approach):

Tuesday:
→ Run all [Signal 1] and [Signal 2] searches across Reddit and Slack
→ Draft replies for the top 3 threads (personalize before sending)
→ DM 1–2 people with fresh Pain Point posts

Thursday:
→ Run X and HN searches
→ Quote tweet or reply to 1–2 X posts
→ Leave 1 substantive HN comment if a relevant thread is active

### Platform rotation rule:
→ Don't hit the same subreddit more than 2× per week
→ Don't use the same reply structure back-to-back in the same community
→ If a DM gets no reply within 5 days → move on, do not follow up
```

### 11.2 Feedback Loop

After 20 interactions (replies + DMs combined), come back with this data:

```
## Check-in (after 20 interactions)

Tell me:
1. How many replies did you send? How many got upvotes or engagement?
2. How many DMs did you send? How many got a response?
3. Which platform had the best response rate?
4. Which signal type led to the best conversations?
5. Any replies that got removed or downvoted? (paste them if you have them)
6. Did any unexpected signals show up — threads or phrases you didn't expect?

Based on your answers, I'll:
→ Reprioritize which signals to focus on
→ Adjust reply tone if you're getting downvoted
→ Identify if the ICP needs narrowing
→ Retire signals that aren't producing results
→ Surface new angles based on what's working
```

### 11.3 When to Expand vs When to Double Down

```
Double down on current channels if:
→ You're getting 1+ meaningful response per 5 DMs
→ Replies are getting upvoted or follow-up questions
→ People are visiting the product URL from outreach

Expand to new communities if:
→ Current communities are saturated (you've replied to most relevant threads)
→ Response rate drops after week 3–4
→ A new signal type appears that points to a different community

Pause and reassess if:
→ 0 responses after 20 DMs
→ Multiple replies getting removed
→ Getting banned or warned in a community
```

---

## 12. Live Post Mode

**Triggered when:** the founder pastes a real post, thread, or tweet they found — without starting a new full spec session.

### 12.1 What It Does

Forget templates. Read the actual post. Generate one custom reply and one custom DM written specifically for that person and their exact situation.

This is not template fill-in. The output should read like it was written by a human who actually read the post.

### 12.2 Input

The founder pastes:
- The original post (verbatim, or a close summary)
- Platform (Reddit / Slack / X / HN)
- Optional: "reply", "DM", or "both"

If product spec was provided earlier in the session, use it. If not, ask for the 3 minimum fields: ONE_LINER, PAIN_PHRASES, OFFER_TYPE.

### 12.3 Process

```
1. Read the post carefully:
   → What is the exact problem or question being expressed?
   → What specific words or phrases did they use?
   → What is their emotional register? (frustrated, curious, comparing options, venting)
   → What platform norms apply?

2. Generate custom reply:
   → Open by addressing their specific situation — use their actual words, not a summary
   → Provide genuine value independent of the product
   → Bridge to product naturally, positioned last
   → Apply platform tone rules strictly

3. Generate custom DM (if applicable):
   → Reference a specific detail from their post — not the topic, their actual words
   → Follow DM structure: Reference → Empathize → Value → Introduce → Low-pressure close
   → Apply platform DM rules

4. Add a one-line note:
   → "Personalize before sending — replace any [bracketed] parts with specifics."
   → Flag if the post is too old to be worth engaging (older than recency window)
   → Flag if the platform's DM culture doesn't support DMing from this type of thread
```

### 12.4 Quality Standard

The output must pass this test: if the person who wrote the original post received this reply or DM, would they think a real person read their post — or would they think it's a template?

If it reads like a template: rewrite it.

---

## 13. Success Criteria

| Criteria | Target |
|----------|--------|
| ICP sharpening | Refined ICP confirmed by founder before playbook generation |
| Start Here quality | Week 1 list is specific enough to execute without re-reading the full playbook |
| Community map coverage | All 4 platforms with specific, relevant communities |
| Signal specificity | Every signal uses product-specific language, no placeholders |
| Template quality | Passes "would you post this on your real account" test |
| Competitor-aware variant | Present for all signal types when COMPETITORS is non-empty |
| Platform differentiation | Reddit / Slack / X / HN tones clearly distinct |
| DM compliance | 100% reference a specific post, 0% from HN threads |
| Live Post Mode quality | Passes "does this read like a real person wrote it" test |
| Cadence realism | Weekly rhythm matches stated founder bandwidth |

---

## 14. Version History

| Version | Change |
|---------|--------|
| v1.0 | Initial concept — Reddit + Slack only |
| v2.3 | Added buying signal library, DM templates, tone rules |
| v3.x | Added Playwright execution, Reddit browser automation |
| v4.0 | Removed all execution. Added X and HN. Pure AI research + draft skill. |
| **v5.0** | **Added ICP Sharpening (Step 0), Output 0 (Start Here), competitor-aware reply variant, Live Post Mode, Output 6 (Ongoing Cadence).** |

---

*End of Specification — first-1000-users v5.0*
