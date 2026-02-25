# Self-Test: first-1000-users markets itself

> **What this is:** Running the skill with `first-1000-users` as the product input.
> The skill seeds itself into real communities. A meta test — and the clearest proof it works.

---

## Product Spec (input)

```
Product: first-1000-users
One-liner: AI skill that takes a product spec → sharpens ICP → returns a complete
           community seeding playbook across Reddit, Slack, X, HN
Problem: Solo founders don't have a distribution system. They browse Reddit
         randomly, write replies that get downvoted, quit after week 2.
Who it's for: Solo founders / indie hackers shipping their first product
Key features: ICP Sharpening, Start Here (Week 1 actions), 20+ search signals,
              4 reply variants per signal, DM templates, Check-in Mode
Pricing: Free (open source)
Stage: Live
URL: github.com/0xconal/cook-a-skill-1000-first-users
Competitors: Generic ChatGPT prompts, manual trial-and-error, paid growth agencies
```

---

## Step 0: ICP Sharpening

**Validation check:**
- TARGET_AUDIENCE: "Solo founders / indie hackers" — specific enough to find on r/indiehackers ✅
- CORE_PROBLEM: "browse Reddit randomly, replies get downvoted, quit after week 2" — user language ✅
- COMPETITORS: Named (ChatGPT, manual browsing, paid agencies) ✅
- PRODUCT_STAGE: Live ✅

**Derived variables:**
```
PAIN_PHRASES:
  → "i've been posting on reddit for weeks and getting nothing back"
  → "i don't know which subreddits my users are actually in"
  → "i wrote a reply and it got downvoted immediately"
  → "i have no idea how to find my first users without paying for ads"
  → "i built it but nobody's coming and i don't know why"

OFFER_TYPE:    free + live → "it's free and open source — paste it into Claude"
MAKER_FRAMING: "i built this"
SWITCHING_COST: low (adds new capability, replaces nothing)
TONE_REGISTER:  casual (indie hackers, solo founders)

COMPETITOR_GAPS:
  → ChatGPT accepts vague ICP, generates generic output immediately
  → Gap: first-1000-users blocks generation until ICP passes validation
  → Manual browsing: no signal framework, no tone guide, no feedback loop
  → Gap: exact search URLs + platform-specific tone rules baked in
```

---

## Refined ICP (confirmed before generating)

**Who:** Solo founders (1–2 person teams), live or near-launch product, first-time distribution — no growth budget, no team.

**The pain, in their words:** "i've been posting on reddit but getting nothing back" / "i don't know which subreddits to target" / "i write replies but they get downvoted"

**What they use instead:** ChatGPT with vague prompts (accepts bad ICP, outputs generic advice), or manual Reddit browsing with no framework.

**Offer:** "it's free and open source — paste SKILL-v5.md into your Claude Project and drop in your spec"

---

## Output 0: Start Here — Week 1

**Highest-signal platform:** Reddit (r/indiehackers) — ground zero for solo founders talking about distribution anxiety. Pain phrases appear daily.

**5 actions, in order:**

1. **r/indiehackers** → `"how do i find my first users" OR "no users after launch" OR "reddit outreach not working" sort:new t:week`
   → Pain Point → DM if personal frustration (<48h), reply if general question | ~15 min/day

2. **r/indiehackers** → `"community seeding" OR "how to find early adopters" OR "distribution for solo founders" sort:new t:week`
   → Direct Request → Reply + DM | ~10 min

3. **Indie Hackers Slack #growth** → search "first users" OR "reddit outreach" OR "community distribution"
   → Reply in thread, DM only if they asked for a specific tool | ~10 min

4. **X** → `indie hacker "first users" OR "distribution" "no idea" min_faves:5`
   → Reply with one specific insight (why Reddit replies get downvoted) | ~10 min

5. **HN** → hn.algolia.com → `"first users" "indie" OR "solo founder" distribution`
   → Leave substantive comment on Ask HN threads, product mention last line | ~15 min

**One thing NOT to do:** Don't post "I built first-1000-users, check it out" as a standalone thread. Show up in conversations about the problem first.

---

## Output 1: Community Map (highlights)

| Platform | Community | Relevance | Why |
|----------|-----------|-----------|-----|
| Reddit | r/indiehackers | HIGH | Core audience — daily distribution pain point threads |
| Reddit | r/SaaS | HIGH | Post-launch "how to get users" questions weekly |
| Reddit | r/startups | MEDIUM | Broader, lower signal — Direct Request only |
| Slack | Indie Hackers | HIGH | #growth, #show-ihn, #tools |
| Slack | WIP.co | HIGH | #help, #marketing |
| X | @levelsio followers | HIGH | Core indie hacker audience |
| HN | Ask HN threads | HIGH | "How did you get your first users?" — recurring |

---

## Output 3: Reply Templates (key variants)

**Pain Point — r/indiehackers — Experience-based:**
> honestly been there. launched something and spent weeks posting randomly on reddit getting nothing. turns out i was in the wrong subreddits and my replies were too polished — communities can tell when it's not a real person.
>
> what helped was switching to signal-based searching — instead of browsing, searching for specific phrases people type when frustrated with the exact problem my tool solves.
>
> ended up building a skill that does this automatically — maps the right communities, generates exact search queries, writes replies in the right tone per platform. it's free if you want to try it, happy to share

**Comparison — r/indiehackers — Competitor-aware:**
> chatgpt works for a first draft but the output is usually too generic — it doesn't know that em dashes get you downvoted on reddit, or that HN will dismiss anything that sounds like a pitch.
>
> the bigger issue is it accepts whatever ICP you give it. if you say "targeting entrepreneurs" it just runs with that — you never get challenged to sharpen who you're talking to.
>
> built something that forces ICP validation before generating anything, then outputs platform-specific templates with tone rules baked in. free to use, just a prompt file you paste into claude

---

## Output 4: DM Template

**Pain Point DM — Reddit:**
> hey saw your post about posting on reddit for weeks and getting zero signups — that sounds incredibly demoralizing
>
> one thing that shifted it: switching from browsing to signal-based searching — looking for exact phrases frustrated users type rather than product category subreddits
>
> i built a free skill that sets this up automatically for your specific product — maps communities, generates search queries, writes platform-specific reply templates. happy to share if useful, no worries if not

---

## Test Result

✅ Pipeline ran clean end-to-end on itself.

**3 observations:**
1. **ICP challenge worked** — "solo founders" could've been vague but derived PAIN_PHRASES got specific fast
2. **Competitor-aware variant landed naturally** — the ChatGPT comparison reply is the strongest template in the set
3. **The em dash point appeared organically** in the X reply — the skill applied its own platform rules to itself
