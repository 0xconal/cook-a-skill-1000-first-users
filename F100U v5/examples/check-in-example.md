# Check-in Mode — Simulated Example

> **Note:** This is a simulated example using realistic Week 1 data for a fictional product.
> It demonstrates how Check-in Mode processes performance data and returns targeted adjustments.

---

## Product

**Patchlog** — a lightweight changelog tool for indie SaaS founders who want to announce updates without maintaining a full blog. Free tier, live product.

---

## Input — Check-in Data (Week 1)

```
1. Replies sent: 12 | Got upvotes or follow-up questions: 4
2. DMs sent: 8 | Got a response: 1
3. Best platform: Reddit (r/indiehackers) — most relevant threads, highest engagement
4. Best signal type: Pain Point — people venting about losing users to silence
5. Downvoted or removed replies: 1 reply on r/SaaS got downvoted (pasted below)
6. Unexpected signals: Found 3 threads about "product-led growth" where founders
   said they don't know how to communicate value after launch — not in original playbook
```

**Downvoted reply (r/SaaS):**
> "The key insight here is that consistent communication is what drives retention — users who feel informed are less likely to churn. We built Patchlog specifically to address this gap in the market."

---

## Output — Check-in Analysis

```
## Check-in: Week 1 Analysis — Patchlog

**Your numbers:**
- Replies: 12 sent → 4 got engagement (33%)
- DMs: 8 sent → 1 response (12.5%)
- Best platform: Reddit (r/indiehackers)
- Best signal: Pain Point

```

---

**What's working — keep doing this:**

→ **Pain Point on r/indiehackers** — 33% reply engagement is above baseline. Threads where founders express "users go quiet after week 1" are consistently matching. Keep running this signal daily.

→ **DM rate (12.5%)** is above threshold — ICP is connecting. The 1 response was from a founder who asked for a demo link, which is a strong buying signal.

---

**What's not working — stop or adjust:**

→ **r/SaaS replies** — 1 downvote after 3 attempts. The community skews more enterprise; your Pain Point framing reads as indie-founder-specific and may not land. Deprioritize for the next 2 weeks.

→ **Workflow Question signal** — 0 DMs, 1 reply with no engagement after 4 attempts. Not enough intent here. Retire for now.

---

**Adjusted signal priorities (next 2 weeks):**
1. **Pain Point** — r/indiehackers → Search: `"users stopped engaging" OR "churn after launch" sort:new t:week`
2. **Direct Request** — r/indiehackers + Slack (Indie Hackers workspace #tools) → "changelog tool" OR "how do you announce updates"
3. **NEW: Post-launch communication** — r/indiehackers → Search: `"product-led growth" "after launch" sort:new t=month` (see New Signal below)

→ **Retire:** Workflow Question signal — insufficient intent after 4 attempts
→ **Deprioritize:** r/SaaS — wrong audience register

---

**Tone fix:** *(triggered — 1 reply downvoted)*

**What went wrong:** The reply used "The key insight here is" (banned Reddit opener) and "gap in the market" (marketing language). It read like a press release, not a peer.

**Corrected version:**
> honestly this is something i've been thinking about too. i had users go quiet for 3 weeks and assumed they were churning — turned out they just didn't know i'd shipped anything. started sending a short update every time i pushed a fix and retention went up noticeably. i built a small tool to make that easier if you want to try it, happy to share

---

**New signal:** *(triggered — unexpected pattern found in 3+ threads)*

**Signal:** Founders asking how to communicate product value after initial launch — not "how do I get users" but "I have users, how do I keep them engaged between big releases"

**Where to find it:**
- r/indiehackers → `"product-led growth" "after launch" sort:new t=month`
- r/SaaS → `"keep users engaged" "between releases" sort:new`

**How to engage:** Reply only (Discussion register — don't DM from these threads). Lead with the specific problem of silence between releases, mention Patchlog as a lightweight solution in the last sentence.

---

**Your next 5 actions (Week 2):**

1. **r/indiehackers** → Search: `"users stopped engaging" OR "churn after launch" sort:new t:week`
   → If Pain Point (<48 hours old): DM with corrected tone (peer framing, no marketing language)
   → Time: ~15 min

2. **r/indiehackers** → Search: `"product-led growth" "after launch" sort:new t=month`
   → New signal — Reply only, no DM. Lead with the silence-between-releases problem.
   → Time: ~10 min

3. **Indie Hackers Slack #tools** → Search: "changelog" OR "announce update"
   → Direct Request signal — Reply + DM if it's a fresh post (<24 hours)
   → Time: ~10 min

4. **r/indiehackers** → Search: `"changelog" OR "how do you tell users" sort:new t:week`
   → Direct Request signal — reply publicly, DM if thread is small (<5 replies)
   → Time: ~15 min

5. **X** → Search: `changelog indie saas min_faves:5`
   → Quote tweet with one specific take about the silence-between-releases problem
   → Time: ~10 min

**Total: ~1 hour for the week's highest-leverage actions.**
