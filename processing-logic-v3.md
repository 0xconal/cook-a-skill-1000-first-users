# PROCESSING LOGIC — v3.0 (OpenClaw Execution)
## Integrated into SKILL.md logic layer

---

## Step 0: Input Validation & Extraction

### 0.1 — Parse Spec into Working Variables

Read product spec, extract into variables. Missing or vague = STOP and ask.

```
PRODUCT_NAME     = [exact name]
ONE_LINER        = [one sentence]
CORE_PROBLEM     = [pain point in user language]
TARGET_AUDIENCE  = [role + stage + context, must be specific]
KEY_FEATURES     = [3-5, ranked by differentiator strength]
PRICING_MODEL    = [free | freemium | paid | open-source]
PRODUCT_STAGE    = [pre-launch | beta | live]
PRODUCT_URL      = [link or "not yet"]
COMPETITORS      = [list with brief notes]
```

### 0.2 — Derive Secondary Variables

```
PAIN_PHRASES     = 3-5 phrases a frustrated real person would type
                   Test: "Would someone actually post this on Reddit?"

AUDIENCE_SIGNALS = Where target audience self-identifies online
                   Subreddit flairs, Slack communities, job titles, bios

SWITCHING_COST   = low | medium | high
                   → Affects CTA aggressiveness in DMs

OFFER_TYPE       = Derived from PRICING_MODEL + PRODUCT_STAGE
                   (see mapping in SKILL.md)

MAKER_FRAMING    = "i built" (maker) or "i've been using" (user)
```

### 0.3 — Initialize Session State (NEW for execution)

```
SESSION_STATE = {
  session_id:        [unique per run]
  product:           [PRODUCT_NAME]
  phase:             [current phase: research | discovery | draft | approve | execute | monitor]
  approval_level:    [1: batch | 2: individual | 3: strict]
  thread_queue:      []      # Discovered threads
  drafts:            []      # Generated drafts
  approved:          []      # Approved messages
  sent:              []      # Executed messages
  contacted_users:   []      # Users already DM'd (loaded from data/contacted_users.json)
  banned_communities: []     # Communities to never engage (loaded from config/banned_list.json)
  rate_limit_state: {
    reddit_replies_this_hour: 0,
    reddit_dms_today: 0,
    reddit_per_sub_today: {},    # {subreddit: count}
    slack_per_channel_hour: {},  # {channel: count}
    slack_dms_today: 0,
    global_this_session: 0,
    global_today: 0,
    last_action_time: null
  }
}
```

### 0.4 — Validate Before Proceeding

```
- [ ] PAIN_PHRASES sound like real frustration, not marketing
- [ ] AUDIENCE_SIGNALS specific enough to map to actual communities
- [ ] OFFER_TYPE matches product's actual stage and pricing
- [ ] At least 2 COMPETITORS identified
- [ ] PRODUCT_URL present (or explicitly "not yet" for pre-launch)
- [ ] contacted_users loaded from persistent storage
- [ ] banned_communities loaded from persistent storage
```

---

## Phase 1 Logic: Research

### Community Map Logic

```
Step 1: Start from AUDIENCE_SIGNALS
  → Generate candidate community list based on where audience hangs out
  → NOT "product is SaaS → list SaaS subreddits"

Step 2: Score each candidate (5 axes, 0-1 each):
  - problem_discussed:  Do PAIN_PHRASES match community topics?
  - audience_present:   Do AUDIENCE_SIGNALS match community demographics?
  - activity_level:     Daily engagement? Active last 7 days?
  - tool_friendly:      Tool recommendations welcome? (not banned)
  - dm_receptive:       Community culture accepts helpful DMs?

Step 3: VERIFY using browser/API (NEW — not just AI knowledge):
  For each candidate scoring 3+/5:
    → Visit subreddit, check last post date
    → Read sidebar rules for self-promo policy
    → Check DM policy if stated
    → For Slack: visit join page, check if open
  Update scores based on verification.
  Mark verification_status: ✅ verified | ⚠️ unverified | ❌ inaccessible

Step 4: Rank by composite score. Include only 3+/5.

Step 5: Derive entry strategy per community:
  HIGH relevance + strict rules → "Contribute 1-2 weeks before mentioning product"
  HIGH relevance + open rules → "Jump in with value-first replies"
  MEDIUM relevance + niche → "Lurk to learn tone, then contribute"

Step 6: DM culture assessment — explicit reasoning:
  Professional/networking community → DMs likely OK
  Help/support community → DMs OK if referencing a specific post
  Discussion/memes community → DMs likely unwelcome
  Sidebar mentions DM policy → Follow it explicitly
```

### Buying Signal Logic

```
Step 1: Take each PAIN_PHRASE, generate 4+ search variations:
  - Frustrated: "I'm so tired of [doing X manually]"
  - Question: "Is there a better way to [do X]?"
  - Wish: "I wish there was something that [does X]"
  - Comparison: "Has anyone tried [competitor] for [use case]?"

Step 2: Assign channel using decision tree (see SKILL.md)

Step 3: Generate SEARCH QUERIES (NEW — these go into actual Reddit search):
  - Each signal produces 1-2 exact search strings
  - Test: paste into Reddit search, do you find relevant results?
  - If not, rewrite the signal

Step 4: Set recency filters:
  - Reply signals: 7 days max thread age
  - DM signals: 3 days max (DMs to old threads feel stalker-ish)
  - High-urgency signals (e.g., "need this by Friday"): 24 hours max

Step 5: Prioritize:
  1. Conversion likelihood: Direct Request > Pain Point > Comparison
  2. Volume: how often does this type of post appear?
  3. Competition: are other products already responding?
```

---

## Phase 2 Logic: Discovery

### Thread Search Process

```
For each signal (highest priority first):
  1. SEARCH Reddit using signal's search query
     → Use praw (Reddit API) if available
     → Fallback: browser automation with playwright-cli
     
  2. FILTER raw results:
     → Age: within recency window for this signal type
     → Status: not locked, not removed, not archived
     → Engagement: at least 1 reply (dead thread = wasted effort)
     → Not duplicate: not already in thread_queue
     → Not contacted: OP not in contacted_users list
     → Not banned: community not in banned_communities list
     
  3. SCORE each thread (0-10):
     signal_match    (0-3): How closely does post match the signal pattern?
     community_rank  (0-2): Community's relevance score from map
     freshness       (0-2): Hours old → 0-6h: 2pt, 6-24h: 1.5pt, 24-72h: 1pt, 72h+: 0.5pt
     engagement      (0-1.5): Reply count → sweet spot is 3-15 replies
     low_competition (0-1.5): Fewer existing product recs = better opportunity
     
  4. DETERMINE action using decision tree:
     → Cross-reference signal type + thread characteristics
     → Verify community allows the recommended action (DM culture check)
     
  5. ADD to thread_queue with full metadata

  6. STOP when thread_queue has 50 items or all signals searched
```

### Thread Scoring Details

```
signal_match scoring:
  3.0 = Post contains exact signal phrase or very close variant
  2.0 = Post clearly describes the same problem/need
  1.0 = Post is tangentially related
  0.5 = Weak match, might not be relevant

freshness scoring:
  2.0 = Posted within last 6 hours (prime time)
  1.5 = 6-24 hours ago (still fresh)
  1.0 = 1-3 days ago (getting stale but OK for replies)
  0.5 = 3-7 days ago (DM only if very high signal match)
  0.0 = Over 7 days (skip unless exceptional match)

engagement scoring:
  1.5 = 3-15 replies (active discussion, not crowded)
  1.0 = 1-3 replies (early, good for visibility)
  0.5 = 15-30 replies (crowded but still visible)
  0.0 = 30+ replies (too crowded, your reply will be buried)
  0.0 = 0 replies (dead thread, OP may have abandoned)

low_competition scoring:
  1.5 = No product recommendations in thread
  1.0 = 1-2 product mentions (competitive but not saturated)
  0.5 = 3-5 product mentions (very competitive)
  0.0 = 5+ product mentions (saturated, skip)
```

---

## Phase 3 Logic: Drafting

### Per-Thread Drafting Process

```
For each thread selected by user:
  1. READ the FULL thread:
     → OP's post (title + body)
     → ALL replies (to understand context and what's been said)
     → OP's replies to comments (to understand what they've tried)
     
  2. EXTRACT thread-specific context:
     → What is OP's SPECIFIC situation? (not generic)
     → What have they already tried? (don't recommend something they rejected)
     → What tone is the thread? (formal? casual? frustrated? analytical?)
     → What does OP seem to value? (speed? price? simplicity? power?)
     
  3. DETERMINE best variant angle:
     → OP tried many things and nothing works → Experience-based
     → OP is comparing specific tools → Comparison-based
     → OP asks "how do I..." → Problem-solving
     
  4. DRAFT the message:
     → Build from structure (Acknowledge → Help → Bridge → Close)
     → Use thread-specific details — not generic filler
     → Match thread tone
     → Keep product mention at the END
     
  5. For DMs, ALSO:
     → Reference a SPECIFIC detail from their post
     → NOT "saw your post about [topic]" — that's generic
     → YES "saw your post about spending 3 hours a day on manual tracking"
     → Calibrate close based on SWITCHING_COST and PRODUCT_STAGE
     
  6. RUN quality gates (see SKILL.md automated checks)
     → Any FAIL → rewrite, don't just flag
     
  7. CROSS-CHECK:
     → Is this draft substantially different from other drafts in this batch?
       If too similar → rewrite with different angle
     → Does this draft respect the community's verified rules?
       If not → adjust or skip
```

### Draft Personalization Depth

```
LEVEL 1 — Surface personalization (MINIMUM):
  Reference the thread topic and OP's name/handle.
  
LEVEL 2 — Context personalization (TARGET):
  Reference specific details from their post.
  Acknowledge what they've already tried.
  Address their specific constraints (budget, team size, timeline).
  
LEVEL 3 — Deep personalization (WHEN POSSIBLE):
  Reference their post history to understand context.
  Connect to other threads they've participated in.
  NOTE: Only do this if visible from public post history.
  NEVER stalk or make the person feel surveilled.
```

---

## Phase 4 Logic: Approval

### Presentation Logic

```
For each draft:
  1. SHOW thread context:
     → Thread title, URL, author, age, reply count
     → Signal match type and score
     → Recommended action (Reply / DM / Both)
     
  2. SHOW draft:
     → Full text as it will appear when posted
     → Quality check results (all should be ✅)
     
  3. SHOW actions:
     → [Approve] [Edit] [Reject] [Skip]
     
  4. If user edits:
     → Re-run quality gates on edited version
     → If edited version fails gates, inform user (but don't block — they may have good reasons)
     
  5. If user rejects with feedback:
     → Log the feedback
     → Use it to calibrate future drafts in this session
     → Pattern: "user prefers shorter replies" → adjust length
     → Pattern: "user wants softer close" → adjust close style
```

### Approval Aggregation

```
After all drafts reviewed:
  1. SUMMARIZE:
     → X approved, Y edited, Z rejected, W skipped
  2. CALCULATE time estimate:
     → Based on rate limits, how long will execution take?
     → E.g., "8 messages will take ~25 minutes (rate limit spacing)"
  3. FINAL CONFIRMATION:
     → "Ready to send X messages. Proceed?"
     → Wait for explicit YES before Phase 5
```

---

## Phase 5 Logic: Execution

### Pre-Send Checks (for EVERY message)

```
Before sending EACH message:
  1. RATE LIMIT CHECK:
     → Is reddit_replies_this_hour < 5?
     → Is time since last same-subreddit action > 2 min?
     → Is reddit_dms_today < 10?
     → Is global_this_session < 20?
     → Is global_today < 30?
     → If ANY fails → queue for later, tell user when
     
  2. THREAD STATUS CHECK:
     → Is thread still unlocked?
     → Is thread still accepting replies?
     → Has thread been significantly modified since we drafted?
     → If STALE → skip, inform user
     
  3. DUPLICATE CHECK:
     → Is this user already in contacted_users? (for DMs)
     → Have we already replied to this thread?
     → If YES → hard block, skip
     
  4. COMMUNITY CHECK:
     → Is this community in banned_communities?
     → Has our activity in this community been flagged recently?
     → If YES → skip, inform user
```

### Send Process

```
For each approved message passing pre-send checks:
  1. EXECUTE:
     → Reddit reply: reddit_post.py --action reply --thread [id] --text [text]
     → Reddit DM: reddit_post.py --action dm --user [user] --text [text]
     → Slack message: slack_post.py --action message --channel [id] --text [text]
     → Slack DM: slack_post.py --action dm --user [id] --text [text]
     
  2. VERIFY:
     → Check response code (200 = success)
     → Verify message appears on the platform
     → If failed → log error, inform user, do NOT retry automatically
     
  3. UPDATE STATE:
     → Add to sent list
     → Update rate_limit_state counters
     → Add user to contacted_users (for DMs)
     → Save action to logs/actions.log
     
  4. WAIT for cooldown:
     → Minimum time between actions based on rate limits
     → Add slight random jitter (±30 seconds) to seem more natural
     
  5. PROCEED to next message or finish
```

### Error Handling

```
HTTP 429 (Rate Limited):
  → Stop immediately
  → Parse retry-after header
  → Inform user: "Rate limited. Resuming in X minutes."
  → Queue remaining messages

HTTP 403 (Forbidden):
  → Stop immediately
  → Check if banned or restricted
  → Inform user: "Access denied in [community]. May be banned."
  → Add community to watch list

HTTP 404 (Thread not found):
  → Skip this message
  → Inform user: "Thread was deleted"
  → Continue with next message

Network error:
  → Retry once after 30 seconds
  → If second failure → skip, inform user
  → Continue with next message

Any unexpected error:
  → Log full error details
  → Skip this message
  → Inform user with error summary
  → Continue with next message (don't stop the whole batch)
```

### Safety Trigger Logic

```
After EVERY action, check:

  removal_detected?
    → Fetch recent post status
    → If post removed by moderator:
      → INCREMENT removal_count for that community
      → PAUSE that community for 48 hours
      → INFORM user
      → If removal_count >= 2 in same community → ADD to banned_communities

  warning_detected?
    → If mod message or warning received:
      → PAUSE ALL ACTIVITY for 24 hours
      → INFORM user immediately
      → Require user to explicitly resume

  ban_detected?
    → If API returns 403 consistently for a subreddit:
      → FULL STOP all activity
      → INFORM user immediately
      → ADD community to banned_communities permanently

  global_removal_rate_check:
    → total_removals / total_sent > 0.10?
      → FULL STOP all activity
      → INFORM user: "Removal rate above 10%. Pausing for review."
      → Require user to review strategy before resuming
```

---

## Phase 6 Logic: Monitor

### Monitoring Process

```
Schedule:
  First 24 hours after sending: check every 30 minutes
  24-72 hours after: check every 2 hours
  72+ hours after: check daily
  After 7 days: stop monitoring (thread is cold)

For each sent message:
  1. CHECK for new replies to our post
  2. CHECK for DM responses
  3. CHECK vote count on our reply
  4. CHECK if our post was removed
  
If response detected:
  → ALERT user immediately
  → SHOW the response context
  → DRAFT a suggested follow-up (if appropriate)
  → PRESENT for approval (NEVER auto-reply)
  → If person says "not interested" → add to do-not-contact, acknowledge gracefully

If negative response (downvotes, negative reply):
  → ALERT user with priority flag
  → Suggest whether to respond or leave it
  → If pattern of negative responses → suggest strategy adjustment
```

### Engagement Metrics Calculation

```
After each monitoring cycle:
  reply_response_rate = responses_to_replies / total_replies_sent
  dm_response_rate = dm_responses / total_dms_sent
  upvote_ratio = (upvotes - downvotes) / total_replies_sent
  removal_rate = removals / total_sent
  conversion_estimate = product_link_clicks / total_sent (if trackable)

Report these to user with trend arrows:
  ↑ improving, → stable, ↓ declining

If reply_response_rate < 10% after 20+ actions:
  → Suggest: "Response rate is low. Consider adjusting draft style or targeting."

If removal_rate > 5%:
  → Suggest: "Some messages are being removed. Review your approach."

If dm_response_rate > 50%:
  → Suggest: "DMs performing well. Consider increasing DM focus."
```

---

## Persistence Between Sessions

```
Save to disk after EVERY phase change:
  data/community_map.json      ← Phase 1 output
  data/signals.json            ← Phase 1 output  
  data/style_guide.json        ← Phase 1 output
  data/thread_queue.json       ← Phase 2 output
  data/drafts/*.json           ← Phase 3 output
  data/approved/*.json         ← Phase 4 output
  data/sent/*.json             ← Phase 5 output
  data/contacted_users.json    ← Updated after every DM
  config/banned_list.json      ← Updated on bans/repeated removals
  logs/actions.log             ← Append after every action
  logs/errors.log              ← Append on errors
  logs/metrics.log             ← Updated after every monitoring cycle

On session resume:
  → Load all persistent state
  → Check: any messages queued but not sent? Resume from there.
  → Check: any monitoring cycles missed? Run them now.
  → Inform user of current state and next recommended action.
```
