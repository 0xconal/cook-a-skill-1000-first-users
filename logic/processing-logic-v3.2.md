# PROCESSING LOGIC — v3.2 (Reddit-Only, Playwright Browser Automation)
## Integrated into SKILL.md logic layer

**Change from v3.1:** All Reddit interactions now go through Playwright browser automation.
praw and Reddit OAuth removed. Error handling updated from HTTP codes to browser states.
Session management and anti-detection logic added.

---

## Step 0: Input Validation & Extraction

### 0.1 — Parse Spec

```
PRODUCT_NAME     = [exact name]
ONE_LINER        = [one sentence]
CORE_PROBLEM     = [pain point in user language]
TARGET_AUDIENCE  = [role + stage + context]
KEY_FEATURES     = [3-5, ranked]
PRICING_MODEL    = [free | freemium | paid | open-source]
PRODUCT_STAGE    = [pre-launch | beta | live]
PRODUCT_URL      = [link or "not yet"]
COMPETITORS      = [list with notes]
```

### 0.2 — Derive Variables

```
PAIN_PHRASES     = 3-5 phrases someone would type on Reddit when frustrated
AUDIENCE_SIGNALS = Subreddit flairs, post patterns, bio keywords
SWITCHING_COST   = low | medium | high
OFFER_TYPE       = Derived from PRICING + STAGE (see SKILL.md mapping)
MAKER_FRAMING    = "i built" (maker) or "i've been using" (user)
```

### 0.3 — Session State

```
SESSION_STATE = {
  session_id:        [unique per run]
  product:           [PRODUCT_NAME]
  phase:             [research | discovery | draft | approve | execute | monitor]
  approval_level:    [1: batch | 2: individual | 3: strict]
  thread_queue:      []
  drafts:            []
  approved:          []
  sent:              []
  contacted_users:   []    # from data/contacted_users.json
  banned_subs:       []    # from config/banned_list.json
  browser_session:   null  # loaded from data/reddit_session.json
  login_count_today: 0     # track re-login attempts
  page_loads: {
    last_10min:  0,        # anti-detection: max 10 per 10 min
    last_hour:   0         # anti-detection: max 30 per hour
  }
  rate_limits: {
    replies_this_hour: 0,
    dms_today: 0,
    per_sub_today: {},     # {subreddit: count}
    session_total: 0,
    day_total: 0,
    last_action_time: null
  }
}
```

### 0.4 — Validate

```
- [ ] PAIN_PHRASES sound like real frustration
- [ ] AUDIENCE_SIGNALS map to specific subreddits
- [ ] OFFER_TYPE matches product stage/pricing
- [ ] At least 2 competitors identified
- [ ] PRODUCT_URL present or explicitly "not yet"
- [ ] contacted_users loaded
- [ ] banned_subs loaded
- [ ] Browser session loaded or login credentials available
```

### 0.5 — Browser Session Initialization (NEW)

```
On every session start:
  1. Check if data/reddit_session.json exists
  2. If YES → load_session() → is_session_valid()
     → If valid → proceed
     → If invalid → re_login_if_needed()
  3. If NO → login(REDDIT_USERNAME, REDDIT_PASSWORD)
              → save session to data/reddit_session.json

Re-login limits:
  - login_count_today >= 3 → STOP, alert user:
    "Too many login attempts today. Please log into Reddit manually
     and restart the skill tomorrow."
  - Never re-login between consecutive actions (use same session throughout)
```

---

## Phase 1: Research

### Subreddit Map Logic

```
Step 1: Start from AUDIENCE_SIGNALS → candidate list
Step 2: Score (5 axes, 0-1): problem_discussed, audience_present,
        activity_level, tool_friendly, dm_receptive
Step 3: VERIFY via Playwright browser:
        verify_subreddit(subreddit_name):
          → Navigate to reddit.com/r/{sub}/
          → Check last post timestamp (extract from post age labels)
          → Navigate to reddit.com/r/{sub}/about/rules (or read sidebar)
          → Extract self-promo rules text
          → Search sidebar for DM/messaging policy mentions
          → Record: verified size, last_post_age, rules_summary, dm_policy
        Mark: ✅ verified | ⚠️ unverified | ❌ inaccessible (sub banned/private)
Step 4: Rank. Include only 3+/5.
Step 5: Entry strategy per sub:
        HIGH + strict → contribute 1-2 weeks first
        HIGH + open → jump in with value-first replies
        MEDIUM → lurk, learn tone, then contribute
Step 6: DM culture:
        Professional/networking → DMs likely OK
        Help/support → DMs OK if referencing a post
        Discussion/memes → DMs unwelcome
        Sidebar policy → follow explicitly
```

**Browser pacing for verification:** Add 2–3 second random delay between subreddit visits.
Max 5 subreddits verified per minute. If CAPTCHA appears during verification → stop Phase 1,
mark remaining subreddits as ⚠️ unverified, continue with what was verified.

### Signal Logic

```
Step 1: PAIN_PHRASES → 4+ search variations each
Step 2: Assign channel via decision tree
Step 3: Generate Reddit search queries
        Format: exact phrases that return real results on reddit.com/search
Step 4: Recency: replies 7d, DMs 3d, urgent 24h
Step 5: Prioritize: conversion × volume × competition
```

---

## Phase 2: Discovery

```
Pre-check: verify browser session valid → re_login_if_needed() if not

For each signal (highest priority first):
  1. SEARCH via Playwright:
     → URL: reddit.com/r/{subreddit}/search?q={query}&sort=new&restrict_sr=1
     → Wait for results to load (2-3s)
     → Extract: title, URL, author, relative age ("6 hours ago"), comment count, score
     → Convert relative age to absolute timestamp
     → If < 5 results → also try: reddit.com/search?q={query}&sort=new (site-wide)
     → Add 1.5–3.5s random delay between searches

  2. FILTER:
     → Age: within recency window (7d for replies, 3d for DMs)
     → Status: not "[removed]", not "[deleted]", not locked icon
     → Engagement: comment_count ≥ 1
     → Not already in thread_queue
     → OP not in contacted_users
     → Subreddit not in banned_subs

  3. SCORE (0-10):
     signal_match (0-3):
       3.0 = title/body contains exact signal phrase
       2.0 = clearly describes same problem
       1.0 = tangentially related
     freshness (0-2):
       2.0 = < 6h, 1.5 = 6-24h, 1.0 = 1-3d, 0.5 = 3-7d
     engagement (0-1.5):
       1.5 = 3-15 replies, 1.0 = 1-3, 0.5 = 15-30, 0 = 30+ or 0
     community_rank (0-2): from map score
     low_competition (0-1.5):
       Only assessable after reading thread — use 1.0 as default at this stage
       Refine to actual score after read_thread() in Phase 3

  4. DETERMINE action (Reply / DM / Both) using signal type

  5. ADD to thread_queue with metadata

  6. STOP at 50 items or all signals searched

  7. CHECK page load limits after each search:
     → page_loads.last_10min >= 10 → wait until window clears before continuing
     → page_loads.last_hour >= 30 → pause Phase 2, inform user of delay
```

**Present to user:**
```
Found [X] threads:

#1 [9.2] r/SaaS — "How did you find your first 100 users?"
   Direct Request | 12h ago | 7 replies | → Reply + DM

#2 [8.7] r/indiehackers — "I built X but have zero users"
   Pain Point 🔥 | 6h ago | 3 replies | → DM first

→ Which threads should I draft for? [All / Select / Top 5]
```

---

## Phase 3: Drafting

```
Pre-check: verify browser session valid

For each selected thread:
  1. READ full thread via read_thread(url):
     → Navigate to thread URL
     → Scroll to load all comments (click "load more" if present)
     → Extract: OP post body, all top-level comments, comment replies, OP's responses
     → Add 1.5-3s delay after page load before extraction

  2. EXTRACT: specific situation, what they tried, tone, values
     → Refine low_competition score now (scan for product mentions in comments)

  3. PICK variant: experience / comparison / problem-solving

  4. DRAFT: structure-first, thread-specific details, product mention at end

  5. For DMs: reference SPECIFIC detail from their post
     BAD: "saw your post about finding users"
     GOOD: "saw your post about spending 3 hours a day browsing r/saas"

  6. Calibrate close by SWITCHING_COST + PRODUCT_STAGE

  7. RUN quality gates → FAIL = rewrite (not just flag)

  8. CROSS-CHECK: different from other drafts? respects verified sub rules?
```

### Personalization Depth

```
Level 1 (minimum): Thread topic + OP handle
Level 2 (target): Specific details, what they tried, their constraints
Level 3 (when possible): Public post history context. NEVER stalk.
  → Only use info visible on their public profile page
  → Never reference post history from a different subreddit context
```

---

## Phase 4: Approval

```
For each draft:
  1. Show: thread context, signal, score, recommended action
  2. Show: full draft text + quality check results (all ✅)
  3. Actions: [Approve] [Edit] [Reject] [Skip]
  4. Edit → re-run quality gates
  5. Reject + feedback → log for future calibration in this session

After all reviewed:
  Summarize: X approved (Y replies, Z DMs), edited, rejected, skipped
  Estimate time: based on rate limits + cooldowns
    Example: "8 messages will take ~35 minutes (rate limit spacing + browser timing)"
  Final confirmation: "Ready to send X messages. Proceed?"
  → Wait for explicit YES
```

---

## Phase 5: Execution

### Pre-Send Checks (every message)

```
1. RATE LIMIT:
   replies_this_hour < 5?
   time since last same-sub action > 2min?
   dms_today < 10?
   time since last DM > 5min?
   session_total < 20?
   day_total < 30?
   → ANY fail = queue for later, tell user when it will send

2. PAGE LOAD LIMITS (anti-detection):
   page_loads.last_10min < 10?
   page_loads.last_hour < 30?
   → Fail = pause, wait for window to clear, then continue

3. THREAD STATUS (re-check before posting):
   → Navigate to thread URL
   → Confirm thread is: not locked, not removed, still accepting replies
   → If stale (thread changed significantly since discovery) → skip, inform user

4. DUPLICATE CHECK:
   → For DMs: username in contacted_users? → HARD BLOCK
   → For replies: already replied to this thread? → HARD BLOCK

5. COMMUNITY CHECK:
   → Subreddit in banned_subs? → skip, inform user
```

### Send Process

```
Post Reply:
  1. Navigate to thread URL
  2. Scroll to find reply box (main post reply or specific comment)
  3. Simulate mouse movement toward reply button
  4. Click reply
  5. Wait 1-2s (simulate reading the reply box)
  6. Type text at human speed (50-120ms per character, random variance)
  7. Wait 2-3s before submitting (simulate review)
  8. Click "Save" / "Submit"
  9. Wait 3s, verify comment appears in thread
  10. Extract comment permalink URL
  11. Log: {timestamp, subreddit, thread_url, comment_url, text, status: "sent"}

Send DM:
  1. Navigate to reddit.com/message/compose?to={username}
  2. Fill Subject field: brief, non-spammy (e.g. "re: your post")
  3. Wait 1s
  4. Type message body at human speed
  5. Wait 2-3s before submitting
  6. Click Send
  7. Wait 3s, verify "message sent" confirmation appears
  8. Log: {timestamp, username, text, status: "sent"}
  9. Add username to contacted_users
  10. Save contacted_users.json immediately
```

### Update State

```
After every successful send:
  → rate_limits counters++
  → page_loads counters++
  → sent[] list updated
  → logs/actions.log appended
  → Wait for cooldown: replies 2min same-sub / DMs 5min + random jitter ±30s
```

### Browser Error Handling

```
CAPTCHA page detected:
  → FULL STOP all execution
  → Save current queue state
  → Alert user: "Reddit is showing a CAPTCHA. Please open your browser,
    solve the CAPTCHA at reddit.com, then restart the skill."
  → Do NOT retry automatically

"You've been doing that too much" banner:
  → Rate limited by Reddit
  → Stop current action
  → Wait 10 minutes minimum
  → Inform user: "Reddit rate limited. Resuming in ~10 minutes."
  → Retry that specific message after wait
  → If same banner again → stop session, queue for next session

"You are banned from this community" message:
  → FULL STOP for that subreddit
  → Add to banned_subs permanently
  → Inform user: "Banned from r/{sub}. All pending messages for this sub cancelled."
  → Continue with other subreddits

Thread not found / 404 page:
  → Thread was deleted since discovery
  → Skip this message, continue
  → Log: status "skipped - thread deleted"

"You must be a member to post" message:
  → Subreddit requires membership
  → Skip for now, flag to user: "r/{sub} requires joining first"
  → Do not add to banned_subs (join manually and retry)

Session expired (redirect to login):
  → Attempt re_login_if_needed()
  → If login_count_today < 3 → re-login, continue
  → If login_count_today >= 3 → FULL STOP, alert user

Network error / page fails to load:
  → Retry once after 30 seconds
  → If second failure → skip this message, continue with next
  → Log: status "skipped - network error"

Any unexpected page state:
  → Take screenshot, log it
  → Skip this message, continue
  → Inform user: "Unexpected page state. Skipped 1 message. Screenshot logged."
```

### Safety Triggers (after EVERY action)

```
Removal detected (on next monitoring cycle):
  → +1 removal count for sub, pause sub 48h
  → 2+ removals same sub → add to banned_subs permanently

Warning message from moderator (in inbox):
  → Pause ALL activity 24h
  → Inform user immediately, require explicit resume

Ban detected (403 or ban message):
  → FULL STOP
  → Inform user
  → Add to banned_subs permanently

Global removal rate > 10%:
  → FULL STOP, force strategy review

CAPTCHA during any phase:
  → FULL STOP, user resolves manually
```

---

## Phase 6: Monitor

```
Schedule:
  First 24h: check every 30 minutes
  24-72h: check every 2 hours
  72h+: check daily
  After 7 days: stop monitoring (thread cold)

Pre-check: verify browser session valid before each monitoring cycle

For each sent reply:
  1. check_comment_status(comment_url):
     → Navigate to comment permalink
     → Read current vote count
     → Check for "[removed]" or "[deleted]" text
     → Count reply comments to our comment
  
For each sent DM:
  2. check_dm_inbox():
     → Navigate to reddit.com/message/inbox
     → Filter for responses to our sent messages
     → Parse sender, message text, timestamp

On response detected:
  → Alert user immediately
  → Show response context (thread or DM)
  → Draft suggested reply (STILL requires approval, never auto-send)
  → If "not interested" / "please don't message me" → add to do-not-contact

On removal detected:
  → Trigger safety logic (see Phase 5 Safety Triggers)

On negative response (hostile reply, "this is spam"):
  → Alert with priority flag
  → Suggest whether to respond or leave it

Metrics (with trend arrows ↑ → ↓):
  reply_response_rate  = responses / total_replies_sent
  dm_response_rate     = dm_responses / total_dms_sent
  upvote_ratio         = (upvotes - downvotes) / total_replies_sent
  removal_rate         = removals / total_sent

Thresholds:
  response < 10% after 20 actions → suggest adjusting approach
  removal > 5% → suggest reviewing strategy
  DM response > 50% → suggest increasing DM focus

Browser pacing for monitoring:
  Add 2-3s delay between each comment/inbox check
  Max 15 page loads per monitoring cycle
  If monitoring cycle exceeds page load limits → spread over next 30 min window
```

---

## Persistence

```
Save after every phase change:
  data/subreddit_map.json       data/signals.json
  data/style_guide.json         data/thread_queue.json
  data/drafts/*.json            data/approved/*.json
  data/sent/*.json              data/contacted_users.json
  data/reddit_session.json      config/banned_list.json
  logs/actions.log              logs/errors.log
  logs/metrics.log

On session resume:
  → Load all persistent state including browser session
  → Verify session still valid → re-login if needed
  → Check: any messages queued but not sent? Resume from there.
  → Check: any monitoring cycles missed? Run them now.
  → Inform user of current state and next recommended action.
```
