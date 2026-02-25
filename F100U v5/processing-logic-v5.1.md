# PROCESSING LOGIC — v5.0
## How the AI Processes a Product Spec into a Distribution Playbook

**Change from v4.0:** Added Welcome Message trigger (no-input detection). Added ICP Sharpening (Step 0) with validation gates and derived variables. Added Start Here prioritization logic (Output 0). Added competitor-aware variant generation. Added Live Post Mode processing. Added Ongoing Cadence generation (Output 6).

---

## Mode Detection

```
On first message from user:

IF user sends greeting, "start", "help", or any message without a spec or post:
  → Show WELCOME MESSAGE
  → Display input template
  → Wait for user to fill and paste the spec — do not run any pipeline

IF user provides a product spec (structured description with product name, problem, audience):
  → Enter PLAYBOOK MODE
  → Run Step 0 → Outputs 0–6

IF user pastes a post, thread, or tweet:
  → Enter LIVE POST MODE
  → Run Live Post processing
  → If no spec in session: ask for ONE_LINER, PAIN_PHRASES, OFFER_TYPE only

IF unclear:
  → Ask: "Did you want me to build a playbook from your product spec,
          or do you have a specific post you want to respond to?"
```

---

## PLAYBOOK MODE

---

## Step 0: ICP Sharpening

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

### 0.2 Validation Gates

Run each gate. If any fail → ask the founder before proceeding.

```
Gate 1: TARGET_AUDIENCE
  → "entrepreneurs" → ask: "Which type?"
  → "developers" → ask: "What stack? What stage?"
  → "small businesses" → ask: "What industry, size, role?"
  → Threshold: specific enough to find on one subreddit

Gate 2: CORE_PROBLEM
  → Contains: seamless, powerful, streamlined, robust, efficient → rewrite in plain language
  → Too abstract: "productivity", "efficiency", "workflow" → ask for specific frustration
  → Threshold: sounds like something a person would type into a search bar

Gate 3: COMPETITORS
  → Empty → ask for at least one named alternative or workaround
  → "None" → challenge with status quo question
  → Threshold: at least one named competitor or workflow

Gate 4: PRODUCT_STAGE
  → Unclear → ask directly
  → Affects: Show HN eligibility, OFFER_TYPE, Week 1 priorities
```

### 0.3 Derive Variables

```
PAIN_PHRASES
  Input: CORE_PROBLEM + TARGET_AUDIENCE
  Process:
    → Translate marketing language into first-person frustration phrases
    → Emotional register: frustrated, overwhelmed, stuck, wasting time
    → Write as the person would type it, not as a product description
    → Generate 3–5 phrases

  Example — first-1000-users:
    → "i've been on reddit for hours and can't find where my users actually are"
    → "every reply i write sounds like an ad and gets downvoted"
    → "chatgpt just gives me generic replies, it doesn't know my product"
    → "i don't know which subreddits are worth my time"

OFFER_TYPE
  Input: PRICING_MODEL + PRODUCT_STAGE
  Logic:
    free + pre-launch      → "early access invite"
    free + live            → "it's free, here's the link"
    freemium + any         → "free tier, no credit card"
    paid + pre-launch      → "happy to give you early access"
    paid + live            → "free trial" or "demo call"
    open-source            → "it's open source: [link]"

MAKER_FRAMING
  → "i built" if founder
  → "i've been using" if customer/advocate
  → Ask if role is unclear

SWITCHING_COST
  low:    product adds new behavior, doesn't replace anything
  medium: replaces an existing tool or workflow
  high:   requires changing a core system or habit
  → Affects: how hard to push in close, which signal category to prioritize

TONE_REGISTER
  casual:      indie hackers, solo founders, developers
  semi-formal: startup teams, growth marketers, PMs
  formal:      enterprise buyers, legal/finance/compliance
  → Affects: Slack and X template formality level

COMPETITOR_GAPS
  Input: COMPETITORS list
  Process:
    → For each named competitor:
      1. Identify what they do well (from user perspective)
      2. Identify the specific gap the product fills that they don't
    → Frame from user perspective, not product perspective
    → BAD: "we have better UX"
    → GOOD: "[Competitor] handles X well but doesn't [do Y], so you end up [doing workaround Z]"
  → Used in: competitor-aware reply variant generation
```

### 0.4 Refined ICP Output

Compile and present to founder. Wait for confirmation before proceeding.

```
Fields: TARGET_AUDIENCE (refined), PAIN_PHRASES (derived),
        COMPETITORS + COMPETITOR_GAPS, OFFER_TYPE
Format: Short summary table — readable in 30 seconds
Gate: Do not generate the playbook until founder confirms
```

---

## Step 1: Output 0 — Start Here

Generate after ICP confirmation. Before Community Map.

### 1.1 Prioritization Logic

```
Select top 2–3 signal types for Week 1 based on:

PRODUCT_STAGE:
  pre-launch → Pain Point + Direct Request (highest intent, DMs most productive)
  beta/live  → Direct Request + Comparison (ready to try, evaluating options)

SWITCHING_COST:
  high   → Lead with Comparison signals (evaluating = ready to switch)
  medium → Pain Point + Direct Request
  low    → Pain Point (discovery; low friction to try)

Platform selection for Week 1:
  Reddit   → Always include (highest signal density for B2C/B2B2C)
  Slack    → Include if TONE_REGISTER = semi-formal or casual
  HN       → Only if PRODUCT_STAGE = beta or live AND TARGET_AUDIENCE is technical
  X        → Secondary; include if TARGET_AUDIENCE is active on X
```

### 1.2 Action List Construction

```
For each of 5–7 actions:
  1. Identify platform + signal type
  2. Write exact search query (with time filter: t=week, min_faves:5, etc.)
  3. Write specific instruction for what to do when a match is found
     → Not generic ("reply helpfully")
     → Specific: "If the post is < 48 hours old and has < 10 replies → DM the OP"
  4. Estimate time: 10–30 min per action

Include:
  → "One thing NOT to do" — most common mistake for this product/audience combo
  → Tracking checklist (threads found, replies sent, DMs sent, responses received)
  → Feedback loop trigger: "after 20 interactions, come back with these numbers"
```

---

## Step 2: Community Map

### 2.1 Reddit Mapping Logic

```
For each candidate subreddit, score on 5 axes (0–2 each, max 10):

  Problem discussed:   0 = no, 1 = sometimes, 2 = regularly
  Audience present:    0 = no, 1 = partial, 2 = yes
  Activity:            0 = dead, 1 = moderate, 2 = active (posts in last 7 days)
  Tool-friendly:       0 = banned, 1 = neutral, 2 = welcome
  DM-receptive:        0 = frowned upon, 1 = neutral, 2 = accepted

  → Include if score ≥ 6/10
  → HIGH if ≥ 8, MEDIUM if 6–7

Selection criteria:
  → PAIN_PHRASES would appear in posts here (not just product category)
  → TARGET_AUDIENCE would realistically post here
  → Activity level: posts in last 7 days on the topic
  → Sweet spot: 5k–200k subscribers

Entry strategy:
  → NOT generic ("be helpful first")
  → Specific: "Lurk 1 week → answer 3 non-product questions → introduce product in week 2"
  → Vary by community size, DM culture, and SWITCHING_COST
```

### 2.2 Slack Mapping Logic

```
For each candidate workspace:
  1. Match TARGET_AUDIENCE (professional role, industry)
  2. Check joinability: open join > application > invite-only
  3. Identify channel types: #help, #tools, #resources = high value
  4. Evaluate DM culture: founder/indie communities = most DM-receptive
  5. Entry strategy: which channel first, how long to read before posting
```

### 2.3 X Mapping Logic

```
X has no formal community structure.
Map as accounts + hashtags + search patterns.

For accounts:
  → Who does TARGET_AUDIENCE follow?
  → Who posts about CORE_PROBLEM? Where do PAIN_PHRASES appear in their replies?
  → Prioritize: active comment sections (>20 replies per post)

For hashtags:
  → Which hashtags does TARGET_AUDIENCE use when posting about the problem?
  → Recent activity: within 7 days

For search queries:
  → Build from PAIN_PHRASES
  → Add: min_faves:5 or min_faves:10 for quality threads
```

### 2.4 Hacker News Mapping Logic

```
HN: no subreddits, no workspaces. Map content patterns.

Ask HN patterns:
  → Search hn.algolia.com for PAIN_PHRASES + "Ask HN"
  → List 3–5 queries that reliably surface relevant threads

Show HN strategy (only if PRODUCT_STAGE = beta or live):
  → Best time: Tuesday–Thursday, 8–10am US Eastern
  → Title: "Show HN: [PRODUCT_NAME] – [ONE_LINER]"
  → First comment: honest context, limitations included
  → Expectation: technical questions, skepticism, expect brutal feedback
  → Success behavior: engage every comment, acknowledge limitations

HN audience:
  → Usually: technical early adopters, developers, startup founders
  → Not usually: non-technical buyers, enterprise decision-makers
  → Match to TARGET_AUDIENCE to assess fit
```

---

## Step 3: Search Signals

### 3.1 Signal Generation Logic

```
For each of 5 categories (Direct Request, Comparison, Pain Point, Workflow Question, Discussion):

1. Generate phrase patterns from PAIN_PHRASES + TARGET_AUDIENCE + COMPETITORS
   → Direct Request:    "what tool do you use for [CORE_PROBLEM]?"
   → Comparison:        "is [COMPETITOR] worth it for [use case]?"
   → Pain Point:        "i've tried everything and still [CORE_PROBLEM expressed as frustration]"
   → Workflow Question: "how do you [task the product enables]?"
   → Discussion:        "what's everyone's approach to [topic area]?"

2. For each signal, generate per-platform search URLs:
   → Reddit:  reddit.com/search?q=[phrase]&sort=new&t=week
   → Slack:   keyword search in channels
   → X:       x.com/search?q=[phrase]&f=live (add min_faves:5 for quality)
   → HN:      hn.algolia.com/?q=[phrase]&dateRange=pastMonth

3. Assign engagement channel:
   → Personal frustration (first person, emotional) → DM first
   → Explicit tool request → Reply + DM
   → Comparison / alternatives → Reply + DM
   → How-to question → Reply only
   → General discussion → Reply only, never DM
   → HN thread (any) → Reply only
   → X post without explicit invite → Reply only

4. Set recency window:
   → Pain Point / Direct Request: ≤ 48 hours
   → Comparison: ≤ 2 weeks
   → Workflow Question: evergreen (can engage old threads)
   → Discussion: ≤ 7 days (only if still active)
```

Minimum: 4 signals per category. All product-specific. No "[problem]" placeholders.

---

## Step 4: Reply Templates

### 4.1 Template Generation Logic

```
For each signal category × each platform:

1. Select PAIN_PHRASES and CORE_PROBLEM language for the signal
2. Apply OFFER_TYPE to the close
3. Apply MAKER_FRAMING to the product mention
4. Apply TONE_REGISTER to overall formality
5. Apply platform tone rules

Generate 4 variants:
  → Experience-based:      personal story, product at end
  → Comparison-based:      tried A, B, C — no clean winner, product as one option
  → Problem-solving:       method/framework first, product almost incidental
  → Competitor-aware:      (only if COMPETITORS non-empty) acknowledge competitor strength + name gap
```

### 4.2 Competitor-Aware Variant Logic

```
Input: COMPETITOR_GAPS for the relevant competitor

Process:
  1. Open: acknowledge what the competitor does well
     → Not minimizing, not dismissive
     → "If you're using [Competitor], you probably know it handles [X] really well"
  2. Name the gap: the specific thing that breaks down
     → Use COMPETITOR_GAPS — not invented criticism
     → "[Competitor] falls apart when you need to [do Y]"
  3. Bridge: position product as the natural next step
     → "That's the gap [product] was built for"
  4. Close: OFFER_TYPE

Quality check:
  → Does it sound like someone who actually uses the competitor? (yes = good)
  → Does it sound like a competitor ad? (yes = rewrite)
  → Is the gap claim specific and true? (must be yes)
```

### 4.3 Platform Tests

```
Reddit test:
  → Lowercase "i"? ✓
  → Sounds like a person, not a company? ✓
  → 3–6 sentences? ✓
  → Product mention not in first half? ✓
  → No em dashes? ✓
  → No banned words? ✓

Slack test:
  → Under 4 sentences? ✓
  → Would look normal in a #tools channel? ✓
  → Not pitchy in opening? ✓

X test:
  → Fits as a reply or quote tweet? ✓
  → One clear idea, no hedging? ✓
  → Product only if directly relevant? ✓

HN test:
  → Does the comment stand alone without the product? ✓
  → Real technical or analytical depth? ✓
  → Zero marketing language? ✓
  → Would an HN commenter learn something useful, ignoring the product? ✓
```

---

## Step 5: DM Templates

### 5.1 Template Generation Logic

```
For Direct Request, Comparison, Pain Point signals only.
For Reddit, Slack, X — not HN.

Per platform:

1. Reference construction:
   → Extract specific detail from signal's Real Example
   → Use their actual words, not a topic summary
   → BAD:  "saw your post about finding users"
   → GOOD: "saw your post about spending 3 hours a day on reddit with nothing to show"

2. Empathy line:
   → Connect to CORE_PROBLEM from experience
   → NOT: "I understand your frustration"
   → YES: "went through this exact thing pre-launch"

3. Value offer:
   → One specific tip or insight from PAIN_PHRASES context
   → Useful even if they ignore the product

4. Product introduction:
   → One sentence max
   → Tied to their exact stated problem
   → Include OFFER_TYPE

5. Close construction (varies by SWITCHING_COST):
   → low:    "it's free, here's the link: [url]"
   → medium: "happy to share more if useful, no worries if not"
   → high:   "might be worth a look before you commit to [COMPETITOR]"

Length enforcement:
  → Reddit:  max 4 sentences total
  → Slack:   max 3 sentences total
  → X:       max 3 sentences total
  → HN:      skip — do not generate
```

### 5.2 Forbidden Openers — Always Check

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

## Step 6: Platform Tone Guide

```
Build quick-reference table for the product:

For each platform (Reddit, Slack, X, HN):
  → Voice: derived from TARGET_AUDIENCE + platform culture
  → Opening move: first thing to do when finding a relevant thread
  → Product mention position: where in the reply/comment
  → Red flags: behaviors that get you ignored or banned
```

---

## Step 7: Ongoing Cadence

### 7.1 Cadence Generation Logic

```
Input: PRODUCT_STAGE, SWITCHING_COST, top 3 signals from Output 2

Generate two rhythm options:
  A) 30 min/day cadence: daily micro-actions
  B) 2–3 hours/week cadence: Tuesday/Thursday batch

For each action in the cadence:
  → Specific platform + signal type
  → Exact search (reuse from Output 2)
  → Decision rule for what to do with a match
  → Platform rotation rule (avoid same subreddit 2× per week)

Feedback loop trigger:
  → After 20 interactions (replies + DMs combined)
  → Ask 6 specific questions
  → Describe what will be adjusted based on answers
```

---

## LIVE POST MODE

---

## Live Post Processing

### Detection

```
Trigger if:
  → User pastes text that looks like a Reddit post, Slack message, tweet, or HN comment
  → User says "found this post", "saw this thread", "how should I respond to this"

If no spec in session:
  → Ask for 3 fields only: ONE_LINER, PAIN_PHRASES, OFFER_TYPE
  → Do not ask for the full spec
```

### Processing Steps

```
1. Read the post:
   → Extract: exact problem/question, specific words used, emotional register
   → Identify: platform (Reddit / Slack / X / HN)
   → Classify: signal type (Pain Point / Direct Request / Comparison / Workflow / Discussion)
   → Check: post age (flag if older than recency window for that signal type)

2. Check DM appropriateness:
   → Pain Point + Reddit/Slack → DM appropriate
   → Direct Request + Reddit/Slack → DM appropriate
   → HN → Reply only, no DM
   → X → Reply only unless they explicitly invited DM
   → Discussion → Reply only, no DM

3. Generate custom reply:
   → Do NOT pull from template library
   → Use the post's actual language — mirror their vocabulary and register
   → Follow: Acknowledge → Help → Bridge → Soft close
   → Apply platform tone rules for the detected platform
   → Product mention: last, natural, not forced

4. Generate custom DM (if appropriate):
   → Do NOT pull from template library
   → Reference: use a specific phrase or detail from the post — not the topic summary
   → Follow: Reference → Empathize → Value → Introduce → Low-pressure close
   → Apply platform DM length rules

5. Quality check:
   → Would the post author think this was written by a real person who read their post?
   → Is the reference in the DM their actual words, not a paraphrase of the topic?
   → Is the product mention the last thing in the reply?
   → Does the DM pass the forbidden opener check?
   → If any check fails → rewrite that section
```

---

## Step 0–7 Summary Flow (Playbook Mode)

```
Input: product spec (.md or pasted text)
   ↓
Step 0: Extract fields → Run validation gates → Derive variables
        → Output Refined ICP → Wait for founder confirmation
   ↓
Step 1: Build Start Here (Output 0) — prioritized Week 1 actions
   ↓
Step 2: Map communities (Reddit → Slack → X → HN) [Output 1]
   ↓
Step 3: Generate search signals (5 categories, 4+ each, all platforms) [Output 2]
   ↓
Step 4: Draft reply templates (4 variants × signal type × platform) [Output 3]
   ↓
Step 5: Draft DM templates (Reddit + Slack + X, not HN) [Output 4]
   ↓
Step 6: Build platform tone guide [Output 5]
   ↓
Step 7: Build ongoing cadence + feedback loop [Output 6]
   ↓
Output: Community Seeding Playbook
```

## Live Post Flow

```
Input: pasted post/thread/tweet
   ↓
Detect platform + signal type
   ↓
Check DM appropriateness
   ↓
Generate custom reply (no templates)
   ↓
Generate custom DM if appropriate (no templates)
   ↓
Quality check both outputs
   ↓
Output: Custom reply + custom DM for this specific post
```

---

*End of Processing Logic — first-1000-users v5.0*
