# PROCESSING LOGIC — v4.0
## How the AI Processes a Product Spec into a Seeding Playbook

**Change from v3.x:** Removed all execution logic (browser automation, session management, rate limiting, posting). This version covers only what the AI does: parse → derive → map → signal → draft.

---

## Step 0: Parse and Validate Input

### 0.1 Extract from Spec

```
PRODUCT_NAME     = exact product name
ONE_LINER        = what it does in one sentence
CORE_PROBLEM     = pain point in user language (not marketing copy)
TARGET_AUDIENCE  = specific role + context
                   BAD:  "entrepreneurs", "developers", "marketers"
                   GOOD: "solo SaaS founders pre-revenue", "indie hackers building side projects"
KEY_FEATURES     = list of 3–5, ranked by differentiation value
PRICING_MODEL    = free | freemium | paid | open-source
PRODUCT_STAGE    = pre-launch | beta | live
PRODUCT_URL      = link or "not yet"
COMPETITORS      = list with notes on what users do today without the product
```

### 0.2 Derive Variables

```
PAIN_PHRASES
  → 3–5 phrases a real person types when frustrated
  → Written in first person, emotional register
  → Not: "looking for community seeding tool"
  → Yes: "i'm spending 3 hours a day on reddit and not finding anything"

  Example — first-1000-users:
  → "i've been on reddit for hours and can't find where my users actually are"
  → "every reply i write sounds like an ad and gets downvoted"
  → "i don't know which subreddits are worth my time"
  → "chatgpt just gives me generic replies, it doesn't know my product"
  → "i wish i knew what to search for instead of just browsing"

AUDIENCE_SIGNALS
  → Platform-specific signals that identify the target user
  → Reddit: subreddit flairs, post patterns, title keywords
  → Slack: workspace type, channel names
  → X: bio keywords, hashtag usage, who they follow
  → HN: comment history, submission patterns

SWITCHING_COST
  → low:    product replaces nothing, adds new behavior
  → medium: replaces an existing tool or workflow
  → high:   requires changing a core system or habit
  → affects how hard to push in the close

OFFER_TYPE (derived from PRICING_MODEL + PRODUCT_STAGE)
  → free + pre-launch      → "early access invite"
  → free + live            → "it's free, here's the link"
  → freemium + any         → "free tier, no credit card"
  → paid + pre-launch      → "happy to give you early access"
  → paid + live            → "free trial" or "demo call"
  → open-source            → "it's open source: [link]"

MAKER_FRAMING
  → "i built" if the user is the founder
  → "i've been using" if the user is a customer
  → Ask if unclear

TONE_REGISTER
  → Derived from TARGET_AUDIENCE formality level
  → casual:      indie hackers, solo founders, developers
  → semi-formal: startup teams, growth marketers
  → formal:      enterprise buyers, legal/finance/compliance roles
```

### 0.3 Validation Gates

Before proceeding to Output 1, check:

```
TARGET_AUDIENCE specific enough?
  → "entrepreneurs" → ask: "Which type? Solo founders, startup employees, agency owners?"
  → "developers" → ask: "Frontend, backend, full-stack? What stage of their career?"

COMPETITORS filled?
  → Empty → ask: "What do users do today without your product?
                  Even 'spreadsheet + manual work' counts."

CORE_PROBLEM in user language?
  → Contains marketing adjectives (seamless, powerful, robust) → rewrite in plain language
  → Too broad ("productivity") → ask: "What specifically frustrates them right now?"

PRODUCT_STAGE unclear?
  → Ask: "Is anyone using this yet, or is it pre-launch?"
```

If validation fails on any critical field → ask the user before generating outputs.

---

## Step 1: Community Map

### 1.1 Reddit Mapping Logic

```
For each candidate subreddit:

1. Match against TARGET_AUDIENCE
   → Does the subreddit description match the audience role?
   → Would TARGET_AUDIENCE realistically post here?

2. Match against PAIN_PHRASES
   → Would PAIN_PHRASES appear in posts here?
   → Is the problem actively discussed (not just tangentially related)?

3. Evaluate engagement quality
   → Community size: too small (<1k) = not worth it, too large (>500k) = hard to stand out
   → Sweet spot: 5k–200k subscribers
   → Activity: posts within last 7 days on the topic

4. Evaluate self-promo rules
   → Tool recommendations: allowed or banned?
   → Flair system: is there a dedicated tools/resources flair?
   → Mod culture: strict enforcement vs relaxed?

5. Evaluate DM culture
   → Does the community norm support DMing after a helpful reply?
   → Are there posts asking for DMs? ("DM me if you want to chat")
   → Community age: older communities tend to be stricter

6. Score on 5 axes (0–2 each, max 10):
   → Problem discussed:      0 = no, 1 = sometimes, 2 = regularly
   → Audience present:       0 = no, 1 = partial, 2 = yes
   → Activity:               0 = dead, 1 = moderate, 2 = active
   → Tool-friendly:          0 = banned, 1 = neutral, 2 = welcome
   → DM-receptive:           0 = frowned upon, 1 = neutral, 2 = accepted

   → Include if score ≥ 6/10
   → Mark as HIGH if ≥ 8, MEDIUM if 6–7

7. Entry strategy
   → NOT generic ("be helpful first")
   → Specific: "lurk 1 week → answer 3 non-product questions → introduce product in week 2"
   → Varies by community size and DM culture
```

Target output: 5–8 subreddits, ranked by score descending.

### 1.2 Slack Mapping Logic

```
For each candidate Slack workspace:

1. Match against TARGET_AUDIENCE
   → Is the workspace name/description for this type of professional?
   → Does the community attract early adopters or decision-makers?

2. Check joinability
   → Public (open join link)? → include
   → Application required? → flag as "apply required"
   → Invite only? → flag as "invite only", deprioritize

3. Identify relevant channels
   → #help, #tools, #resources → high value
   → #jobs, #showcase, #random → medium value
   → Channels named after CORE_PROBLEM topic → highest value

4. Evaluate DM culture
   → Slack workspaces with active tool discussions = DM-friendly
   → Enterprise-style workspaces = more formal, DM with care
   → Founder/indie communities = most DM-receptive

5. Entry strategy
   → Specific: which channel to join first, how long to read before posting
```

Target output: 3–5 workspaces, ranked by relevance.

### 1.3 X (Twitter) Mapping Logic

```
X has no formal community structure.
Map as accounts + hashtags + search patterns.

For accounts:
→ Who does TARGET_AUDIENCE follow?
→ Who posts about CORE_PROBLEM regularly?
→ Who has active replies where PAIN_PHRASES appear?
→ Prioritize accounts with engaged comment sections (>20 replies per post)

For hashtags:
→ Which hashtags does TARGET_AUDIENCE use when posting about the problem?
→ Which hashtags appear in posts containing PAIN_PHRASES?
→ Prioritize hashtags with recent activity (within 7 days)

For search queries:
→ Build queries using PAIN_PHRASES
→ Filter: min_faves:10 to find threads with traction
→ Look for quote tweet chains — these are high-engagement discussion clusters

Engagement approach per entry:
→ Account: reply to threads where PAIN_PHRASES appear in comments
→ Hashtag: search + reply to relevant posts
→ Search query: use to find threads, then decide reply vs quote tweet
```

Target output: 5–8 entries (mix of accounts, hashtags, queries).

### 1.4 Hacker News Mapping Logic

```
HN has no subreddits or workspaces.
Map as content patterns.

Ask HN patterns:
→ Search hn.algolia.com for PAIN_PHRASES + "Ask HN"
→ Example: "Ask HN: how do you find your first users"
→ List 3–5 search queries that reliably surface relevant threads

Show HN strategy:
→ Only if PRODUCT_STAGE = beta or live
→ Best time: Tuesday–Thursday, 8–10am US Eastern
→ Title format: "Show HN: [PRODUCT_NAME] – [ONE_LINER]"
→ First comment: honest context about what you built and why
→ What to expect: technical questions, skepticism, sometimes brutal feedback
→ What works: transparency about limitations, engaging with every comment

Comment opportunities:
→ Types of threads where a helpful comment + product mention is appropriate
→ Example: "Ask HN: tools for X" → add substantive answer, mention product at end
→ Example: threads about the problem space → add analysis, product as footnote

HN audience profile:
→ Identify which segment of TARGET_AUDIENCE is on HN
→ Usually: technical early adopters, developers, startup founders
→ Not usually: non-technical buyers, enterprise decision-makers
```

---

## Step 2: Search Signals

### 2.1 Signal Generation Logic

```
For each of 5 categories (Direct Request, Comparison, Pain Point, Workflow Question, Discussion):

1. Generate phrase patterns from PAIN_PHRASES + TARGET_AUDIENCE + COMPETITORS
   → Direct Request: "what tool do you use for [CORE_PROBLEM]?"
   → Comparison: "is [COMPETITOR] worth it for [use case]?"
   → Pain Point: "i've tried everything and still [CORE_PROBLEM]"
   → Workflow Question: "how do you [task the product enables]?"
   → Discussion: "what's everyone's approach to [topic area]?"

2. For each signal, generate per-platform search queries:
   → Reddit:  site:reddit.com "[phrase]" or use reddit.com/search
   → Slack:   keyword search within channels
   → X:       twitter.com/search "[phrase]" (add min_faves:5 for quality)
   → HN:      hn.algolia.com "[phrase]"

3. Assign engagement channel:
   → Personal frustration (first person, emotional) → DM first
   → Explicit tool request → Reply + DM
   → Comparison / alternatives → Reply + DM
   → How-to question → Reply only
   → General discussion → Reply only
   → HN thread (any) → Reply only
   → X post without explicit invite → Reply only

4. Identify strongest platform:
   → Where does this signal appear most frequently?
   → Where does TARGET_AUDIENCE discuss this most?

5. Set recency window:
   → Pain Point / Direct Request: reply within 48 hours ideally
   → Comparison: up to 2 weeks
   → Workflow Question: can engage older threads (evergreen)
   → Discussion: only engage if thread is active (within 7 days)
```

Minimum: 4 signals per category. All product-specific. No "[problem]" placeholders.

**Example output — first-1000-users, Pain Point signal:**

```
Signal: Pain Point
Pattern: "spending hours on reddit/slack without finding the right threads"

Reddit search: "wasting time reddit browsing founder users"
Slack search: "reddit research time"
X search: "reddit hours browsing founder users min_faves:5"
HN search: "find first users reddit effort"

Real example: "i've been spending like 2-3 hours every morning on reddit hoping to stumble on someone asking about this. it's not working and i'm running out of runway to figure this out"
→ Engagement: DM first
→ Strongest platform: Reddit
→ Recency window: 48 hours
```

---

## Step 3: Reply Templates

### 3.1 Template Generation Logic

```
For each signal category × each platform:

1. Select the signal's PAIN_PHRASES and CORE_PROBLEM language
2. Apply OFFER_TYPE to the close
3. Apply MAKER_FRAMING to the product mention
4. Apply platform tone rules (see SKILL.md Section 6.3)

Generate 3 variants:
→ Experience-based: open with personal story, product at end
→ Comparison-based: open with "tried A, B, C", product as one option
→ Problem-solving: open with method/framework, product almost incidental

Quality checks per template:
→ Remove all em dashes
→ Remove all banned words
→ Verify product mention is not in first half of reply
→ Verify reply adds value without the product mention
→ Verify tone matches platform (run platform test)
```

### 3.2 Platform Test

```
Reddit test:
→ Does it use lowercase "i"?
→ Does it sound like a person, not a company?
→ Is it 3–6 sentences?
→ Does the product mention feel incidental?

Slack test:
→ Is it under 4 sentences?
→ Would it look normal in a #tools channel?

X test:
→ Does it fit as a reply or quote tweet?
→ Is there one clear idea, not multiple?

HN test:
→ Does the comment stand alone without the product?
→ Is there real technical/analytical depth?
→ Does it avoid all marketing language?
→ Would an HN commenter learn something useful from it, ignoring the product mention?
```

---

## Step 4: DM Templates

### 4.1 Template Generation Logic

```
For Direct Request, Comparison, Pain Point signals only.

Per platform (Reddit, Slack, X — not HN):

1. Reference construction
   → Extract specific detail from signal's Real Example
   → Use their actual words, not a summary of topic
   → BAD:  "saw your post about finding users"
   → GOOD: "saw your post about spending 3 hours a day on reddit with nothing to show"

2. Empathy line
   → Connect to CORE_PROBLEM from personal experience
   → NOT: "I understand your frustration"
   → YES: "went through this exact thing when we were pre-launch"

3. Value offer
   → One specific tip or insight from PAIN_PHRASES context
   → Something useful even if they ignore the product

4. Product introduction
   → One sentence max
   → Tied to their exact stated problem
   → Include OFFER_TYPE

5. Close construction
   → Vary by SWITCHING_COST:
     → low:    "it's free, here's the link: [url]"
     → medium: "happy to share more if useful, no worries if not"
     → high:   "might be worth a look before you commit to [COMPETITOR]"

Platform length enforcement:
→ Reddit:  max 4 sentences total
→ Slack:   max 3 sentences total
→ X:       max 3 sentences total
→ HN:      skip — do not generate
```

### 4.2 Forbidden Openers (always check)

```
Never start DMs with:
→ "I hope this finds you well"
→ "I wanted to reach out"
→ "I noticed that"
→ "I came across your post"
→ "I thought you might be interested"
→ "As a fellow [role]"
→ "I'm reaching out because"
```

---

## Step 5: Platform Tone Guide

### 5.1 Generation Logic

```
Build a quick-reference table customized for the product:

For each platform (Reddit, Slack, X, HN):
→ Voice: derived from TARGET_AUDIENCE + platform culture
→ Opening move: first thing to do when you find a relevant thread
→ Product mention position: where in the reply/comment
→ Red flags: behaviors that will get you ignored or banned
→ One example line: a sample sentence that sounds right for this product on this platform
```

---

## Step 0–5 Summary Flow

```
Input: product spec (.md or pasted text)
   ↓
Step 0: Parse fields → Derive variables → Validate
   ↓
Step 1: Map communities (Reddit → Slack → X → HN)
   ↓
Step 2: Generate buying signals (5 categories, 4+ each, all platforms)
   ↓
Step 3: Draft reply templates (3 variants × signal type × platform)
   ↓
Step 4: Draft DM templates (Reddit + Slack + X, not HN)
   ↓
Step 5: Build platform tone guide
   ↓
Output: Community Seeding Playbook
```

---

*End of Processing Logic — first-1000-users v4.0*
