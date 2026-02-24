# SPEC DOCUMENT — `first-1000-users`

**An OpenClaw Skill for AI-Powered Reddit Seeding**

Platform: OpenClaw (ClawBot)
Version 3.2 | February 2026
Status: Draft — Pending Supervisor Approval

**Change from v3.1:** Replaced Reddit API (praw + OAuth) with Playwright browser automation.
No API registration or approval required. Only a real Reddit account (username + password) needed.

---

## 1. Overview

`first-1000-users` is an OpenClaw skill that helps founders seed their product into real Reddit conversations. The AI agent analyzes a product spec, maps relevant subreddits, finds real threads, drafts personalized replies and DMs, and executes approved outreach via browser automation.

### 1.1 Core Idea

Find people who are **already looking** for what you built on Reddit. Show up with a helpful reply in the thread and a thoughtful DM to the person who raised the problem. AI handles research, discovery, and drafting. Human approves every message before it goes out.

### 1.2 Two Engagement Channels

| Channel | When to Use | Conversion |
|---------|------------|------------|
| **Public Reply** | Thread is active, multiple people have the problem | Lower per-reply, visible to many |
| **Direct Message** | One person clearly has the pain point | Higher per-message, 1-on-1 |

### 1.3 What This Skill Is

- ✅ AI agent: researches subreddits, finds real threads, drafts personalized outreach
- ✅ Executes approved outreach on Reddit via Playwright browser automation
- ✅ Human-in-the-loop: every message requires explicit approval
- ✅ No API registration required — uses your existing Reddit account

### 1.4 What This Skill Is NOT

- ❌ Not fully autonomous — every action requires human approval
- ❌ Not a spam tool — hard-coded rate limits and anti-abuse protections
- ❌ Not a fake-account system — user's real Reddit account only
- ❌ Not using the Reddit API — browser automation only (no OAuth app needed)

### 1.5 Execution Model

```
AI does:    Research, Search, Draft, Post (after approval), Monitor
Human does: Approve, Edit, Reject, Handle follow-up conversations

Rule: AI NEVER sends anything without explicit approval
```

---

## 2. How It Works — 6-Phase Pipeline

```
Phase 1: RESEARCH   → Analyze spec, map subreddits, generate signals
Phase 2: DISCOVERY  → Search Reddit for real matching threads (via browser)
Phase 3: DRAFT      → Personalized reply/DM per thread
Phase 4: APPROVE    → Human reviews each draft [GATE]
Phase 5: EXECUTE    → Post approved messages (via browser)
Phase 6: MONITOR    → Track engagement, alert on responses (via browser)
```

### 2.1 Input

A `.md` file describing the product:

| Section | What to Include |
|---------|----------------|
| Product name & one-liner | What it is in one sentence |
| Problem it solves | Pain point in user's language |
| Who it's for | Target audience (specific) |
| Key features | Top 3–5 differentiators |
| Pricing | Free / freemium / paid |
| Current stage | Pre-launch / beta / live |
| Product URL | Link or "not yet" |
| Competitors | What people use today instead |

### 2.2 Outputs per Phase

| Phase | Output |
|-------|--------|
| 1. Research | Subreddit Map, Buying Signals, Style Guide |
| 2. Discovery | Ranked Thread Queue (real threads) |
| 3. Draft | Personalized messages per thread |
| 4. Approve | Human-vetted message queue |
| 5. Execute | Action log with statuses |
| 6. Monitor | Engagement report |

---

## 3. Phase 1: Research

### 3.1 Subreddit Map

Ranked list of subreddits, verified via Playwright browser:
- Verified size, relevance score, self-promo rules, DM culture, entry strategy
- Scored on 5 axes: problem discussed, audience present, activity, tool-friendly, DM-receptive
- Only include 3+/5 scores
- Target: **5–8 subreddits**

### 3.2 Buying Signal Library

Categorized search phrases, specific to the product:
- 5 categories: Direct Request → Comparison → Pain Point → Workflow Question → Discussion
- Each signal includes: pattern, Reddit search query, real example, engagement channel, recency filter
- At least 4 signals per category

### 3.3 Style Guide

Derived variables: OFFER_TYPE, MAKER_FRAMING, SWITCHING_COST, tone notes.

---

## 4. Phase 2: Discovery

Search Reddit via browser for real threads matching signals. Navigate to
`reddit.com/r/{sub}/search?q={query}&sort=new&restrict_sr=1`, extract results,
filter by recency/engagement/competition. Score 0-10. Present ranked Thread Queue to user.

Max 50 threads per session. Refresh daily.

---

## 5. Phase 3: Draft

Read full thread via browser → draft personalized message (not template fill-in).

**Reply structure:** Acknowledge → Help → Bridge → Soft close
**DM structure:** Reference → Empathize → Value → Introduce → Low-pressure close

Automated quality gates run before presenting to user.

**Tone:** Lowercase "i", no em dashes, human filler, messy numbers, product mention at end. 3-6 sentences for replies, 3-4 max for DMs.

---

## 6. Phase 4: Approve

Human reviews each draft: Approve / Edit / Reject / Skip. Final confirmation before execution.

---

## 7. Phase 5: Execute

Post via Playwright browser automation. Rate limits enforced:

| Action | Limit |
|--------|-------|
| Replies | 5/hour |
| Same subreddit | 2/day, 2 min cooldown |
| DMs | 10/day, 5 min between |
| Per session | 20 max |
| Per day | 30 max |

**Safety triggers:** removal → pause 48h, warning → pause all 24h, ban → full stop,
removal rate > 10% → full stop, CAPTCHA → full stop (user resolves manually).

---

## 8. Phase 6: Monitor

Track responses every 30 min (first 24h), then daily. Navigate to comment permalinks
and reddit.com/message/inbox to check responses. Alert on replies. Draft follow-ups
(still require approval). Never auto-reply. Never follow up on unanswered DMs.

---

## 9. Ethical Guidelines

### 9.1 Hard-Coded Rules

- Human approval required for every message
- One DM per person (contacted_users database)
- Rate limits cannot be overridden
- No fake accounts
- Every message personalized per thread
- Bans permanently block community
- No follow-up DMs on no-reply
- Auto-pause on removals/warnings

### 9.2 Disclosure

User is the sender. AI is a drafting and automation tool. User approves every message.
Analogous to having an assistant draft and send messages on your behalf.
If Reddit rules change to require AI disclosure, skill should be updated.

---

## 10. Technical Requirements

### 10.1 Skill Structure

```
~/.openclaw/skills/first-1000-users/
├── SKILL.md                    # Main instructions (OpenClaw format)
├── scripts/
│   ├── reddit_browser.py       # All Reddit actions via Playwright
│   └── rate_limiter.py         # Enforce rate limits
├── config/
│   ├── rate_limits.json
│   ├── approval_level.json
│   └── banned_list.json
├── data/
│   ├── product_spec.md
│   ├── subreddit_map.json
│   ├── signals.json
│   ├── style_guide.json
│   ├── thread_queue.json
│   ├── reddit_session.json     # Browser session/cookies (NEW)
│   ├── drafts/
│   ├── approved/
│   ├── sent/
│   └── contacted_users.json
└── logs/
    ├── actions.log
    ├── errors.log
    └── metrics.log
```

**Key change from v3.1:** Five separate scripts (`reddit_search.py`, `reddit_post.py`,
`reddit_verify.py`, `engagement_tracker.py`, `rate_limiter.py`) are consolidated into
two: `reddit_browser.py` (all browser actions) and `rate_limiter.py` (unchanged logic).

### 10.2 Dependencies

```yaml
requires:
  env:
    - REDDIT_USERNAME    # Your Reddit username (no @ prefix)
    - REDDIT_PASSWORD    # Your Reddit account password
  bins:
    - python3
    - playwright-cli
  python:
    - playwright
```

**Removed from v3.1:** `REDDIT_CLIENT_ID`, `REDDIT_CLIENT_SECRET`, `praw`
**No OAuth app registration required.**

### 10.3 reddit_browser.py — Function Reference

All Reddit interactions go through a single browser session managed by this script.

```python
# Session
login(username, password)               # Login once, save session to data/reddit_session.json
load_session()                          # Restore session from file
is_session_valid()                      # Check if still logged in
re_login_if_needed()                    # Auto re-login if session expired

# Phase 1 — Research
verify_subreddit(subreddit_name)        # Visit sub, read sidebar rules, check activity
  → returns: {size, active, rules_text, dm_policy, last_post_age}

# Phase 2 — Discovery
search_reddit(query, subreddit, sort)   # Navigate search URL, extract results
  → returns: [{title, url, author, age, comment_count, score}]
read_thread(url)                        # Load full thread, extract all comments + OP replies
  → returns: {op_post, all_comments, op_replies_to_comments}

# Phase 5 — Execute
post_reply(thread_url, text)            # Navigate to thread, find reply box, type, submit
  → returns: {success, comment_url, error}
send_dm(username, text)                 # Navigate to /message/compose, fill, send
  → returns: {success, error}

# Phase 6 — Monitor
check_comment_status(comment_url)       # Navigate to comment, check votes + removal
  → returns: {vote_count, is_removed, is_deleted, reply_count}
check_dm_inbox()                        # Navigate to /message/inbox, parse unread
  → returns: [{from_user, message, thread_url, timestamp}]
```

### 10.4 Session Management

```
Login policy:
  - Login once per day maximum
  - Save browser cookies to data/reddit_session.json after login
  - Load session on each script invocation (avoid repeated logins)
  - Check session validity before Phase 2, 5, 6 actions
  - If session expired → re-login once, update session file
  - If re-login fails → alert user, stop execution

Re-login limits:
  - Max 3 re-logins per 24 hours (Reddit flags frequent logins as suspicious)
  - If limit reached → stop, alert user: "Session issue. Please log into Reddit manually."
```

### 10.5 Anti-Detection Measures

Reddit actively detects and blocks automated browsers. These measures reduce detection risk:

```
Browser config:
  - Run non-headless (headless=False) — headless browsers are more easily detected
  - Use a real user-agent string matching a current Chrome/Firefox version
  - Do not run multiple browser instances simultaneously

Timing:
  - Random delay 1.5–3.5 seconds between page loads (avoid robotic regularity)
  - Simulate human typing speed when filling text fields (50–120ms per character)
  - Simulate brief mouse movement before clicking buttons
  - Never submit forms faster than a human realistically could (min 3 seconds per form)

Request limits (separate from business rate limits):
  - Max 10 page loads per 10 minutes
  - Max 30 page loads per hour
  - If CAPTCHA or verification page appears → FULL STOP, alert user to resolve manually

Session behavior:
  - Do not clear cookies between actions (persistent session looks more human)
  - Navigate naturally: go to thread page, scroll, then click reply (not direct DOM injection)
```

---

## 11. Build Plan — 2-Day Sprint

### Day 1: Research + Discovery + Draft (Phase 1-3)

**Morning:**
- Set up OpenClaw skill directory and SKILL.md with YAML frontmatter
- Implement Step 0: spec parsing, variable extraction, validation
- Write `reddit_browser.py` — login, session management, `verify_subreddit()`
- Implement Phase 1: subreddit mapping with live browser verification, signal generation, style guide
- Test: feed product spec → verify research output quality

**Afternoon:**
- Add `search_reddit()` and `read_thread()` to `reddit_browser.py`
- Implement Phase 2: thread discovery, scoring, thread queue
- Implement Phase 3: per-thread drafting with quality gates
- Implement approval presentation format
- Test: signal → real threads (browser search) → personalized drafts → approval flow

**Day 1 Deliverable:** Working pipeline from spec input through draft approval. Browser search confirmed working.

### Day 2: Execute + Monitor + Safety (Phase 4-6)

**Morning:**
- Add `post_reply()` and `send_dm()` to `reddit_browser.py`
- Implement `rate_limiter.py` with hard limits
- Build contacted_users database for duplicate prevention
- Implement anti-detection timing in all browser actions
- Implement safety triggers: CAPTCHA detection, removal detection, ban detection
- Implement error handling (CAPTCHA, session expired, thread deleted, rate limited banner)
- Test: approved draft → browser posts reply → verify visible → log

**Afternoon:**
- Add `check_comment_status()` and `check_dm_inbox()` to `reddit_browser.py`
- Implement Phase 6 monitoring loop
- Full pipeline test: spec → research → discover → draft → approve → execute → monitor
- Test edge cases: thread locked, CAPTCHA, session expired, post removed, rate limited
- Write documentation and example output
- Package and submit

**Day 2 Deliverable:** Complete skill with browser execution, monitoring, and safety.

---

## 12. Success Criteria

| Criteria | Target |
|----------|--------|
| Subreddit relevance | 4/5 top suggestions are active + on-topic |
| Signal accuracy | 5+ real threads per top 3 signals |
| Thread discovery | 10+ threads per session |
| Draft quality | 90%+ pass automated gates |
| Human approval rate | 70%+ approved without major edits |
| Execution success | 95%+ messages post successfully (no CAPTCHA, no session issues) |
| Engagement | 30%+ reply response rate |
| Safety | 0 bans, < 5% removal rate in first 50 actions |

---

*End of Specification — first-1000-users v3.2 (Reddit-only, Playwright browser automation)*
