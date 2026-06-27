# Etch — Launch Plan

## Launch Strategy: "The 2am Launch"

Don't launch like a SaaS product. Don't launch like a wellness app. Launch like a vibe.

### Core Narrative

*"Every app tracks you. Your likes, your location, your messages, your purchases. Etch is the one app that physically cannot track you. Your entries are plain files on your device. No server. No analytics. No algorithm. Just you and your mind at 2am."*

---

## Pre-Launch (2 weeks before)

### Week -2
| Day | Task |
|-----|------|
| Mon | Post first TikTok: close-up of app in dark mode, typing at night, wax seal. Caption: "the one app the algorithm can't see" |
| Wed | Post "Why Etch has no streaks" essay on Reddit (r/GenZ, r/journaling, r/mentalhealth) |
| Fri | Share dev journey on TikTok: building the Color Cloud, testing vibe gradients |

### Week -1
| Day | Task |
|-----|------|
| Mon | Open TestFlight beta. Recruit 50 Gen Z testers from TikTok, Reddit, Discord communities |
| Wed | Prepare App Store listing. Screenshots all in dark mode, 2am aesthetic. Keywords: journal, private, shadow work, no subscription |
| Thu | Build landing page (simple HTML, dark, minimal): "the algorithm can't see this" + App Store link |
| Fri | Prepare Product Hunt listing. Prepare Show HN draft. Dry run. |

---

## Launch Day (Saturday EST)

**Why Saturday?** Gen Z is most active on TikTok on weekends. Less corporate noise on HN. The 2am brand works — Saturday night IS 2am.

### Schedule (EST → SGT)

| Time EST | Time SGT | Action | Where |
|----------|----------|--------|-------|
| 08:00 | 21:00 | Show HN post | Hacker News |
| 08:05 | 21:05 | Product Hunt launch | producthunt.com |
| 08:30 | 21:30 | Reddit posts | r/GenZ, r/journaling, r/mentalhealth, r/privacy |
| 09:00 | 22:00 | TikTok launch video | TikTok (the wax seal ASMR video) |
| 10:00 | 23:00 | Color Cloud / Aura video | TikTok ("my month in color") |
| 10:00+ | 23:00+ | Respond to EVERY comment | All platforms |

### Show HN Post

**Title:** `Show HN: I built a journaling app with zero network code`

**Body:**
```
Every journaling app wants $35/year and tracks your data. I built one that has no server, no analytics, no tracking — and no subscription.

Etch saves entries as plain .md files to your own iCloud Drive. The app literally has no network code. No URLSession. No push notifications. No telemetry. Your thoughts cannot leave your device.

It's designed for Gen Z — people who journal at 2am to decompress, not to build a legacy archive. No streaks. No gratitude prompts. No "corporate wellness" vibes.

Features:
- Hold-to-enter opening ritual (psychological boundary from the noise)
- Vibe slider (set emotional temperature: Chaotic/Static/Melancholy/Glowing)
- Wax seal save animation (saving feels like closure, not a database write)
- Burn folder (entries that self-delete in 24 hours — catharsis without permanence)
- Redact tool (permanently black out your own words)
- Color cloud (monthly mood as abstract art — shareable, zero private data)

$24.99 one-time. No subscription. No dark patterns. If I abandon this, I open-source the code.

Built with SwiftUI. Uses your existing iCloud storage. No server required.

I'd love feedback, especially on the file system approach and the vibe slider UX.
```

### Product Hunt

**Title:** `Etch — A journal that doesn't grade you`
**Tagline:** `The place the algorithm can't see. No subscription. No tracking. No streaks.`

### Reddit Posts

**r/GenZ:** *"I'm so tired of apps tracking everything. I built a journaling app with zero network code. Your entries are plain files on your device. No server can see them. No algorithm can read them."*

**r/journaling:** *"Every journaling app is built for millennials (legacy, gratitude, streaks). I built one for Gen Z — hold to enter, set your vibe, write whatever, wax seal closes it. No streaks. No '3 things you're grateful for.' No $35/year."*

**r/privacy:** *"I built a journaling app with literally zero network code. No URLSession. No analytics. No backend. Entries are plain .md files in your iCloud. The app cannot send data anywhere even if it wanted to."*

**r/mentalhealth:** *"Journaling apps have a toxicity problem: streaks, gratitude prompts, 'wellness coaching.' I built Etch to be the opposite — a sanctuary, not a productivity tool. No streaks. No toxic positivity. Just a private space to process your day."*

---

## Post-Launch (Weeks 1-4)

| Week | Focus | Tactics |
|------|-------|---------|
| 1 | TikTok momentum | Post 3-5 videos: wax seal ASMR, vibe slider, color cloud, "my month in color" trend. Reply to every comment. |
| 2 | Community engagement | Reddit follow-ups, respond to reviews, fix bugs from TestFlight feedback |
| 3 | Micro-influencer outreach | Pitch 10-15 mental health / shadow work TikTok creators (1k-50k followers). Free Pro + affiliate. |
| 4 | Content series | "Anti-journal prompts" series on TikTok — post prompt screenshots that resonate |

### Metrics to Watch

| Metric | Target (Month 1) |
|--------|-----------------|
| App downloads | 5,000 |
| Free → Pro conversion | 3-5% (150-250 sales) |
| TikTok video views | 100K+ across all videos |
| App Store rating | 4.5+ stars |
| 1-star reviews mentioning subscription | 0 (we have none) |

### What NOT to Track

- User behavior analytics (we don't have analytics — that's the point)
- Retention metrics that imply streak/guilt mechanics
- DAU/MAU in the traditional sense (Etch isn't a daily-use app by design — some people journal once a week, and that's fine)

---

## Launch Assets Checklist

| Asset | Status |
|-------|--------|
| App Store screenshots (dark mode, 2am aesthetic) | Needed |
| App Store description (Gen Z voice, anti-corporate) | Needed |
| App icon (minimal, warm on black) | Needed |
| TikTok launch video (wax seal ASMR) | Needed |
| TikTok Color Cloud video | Needed |
| Landing page (simple HTML, dark) | Needed |
| Product Hunt assets (gallery, description) | Needed |
| Privacy policy (simple, honest) | Needed |
| End of Life guarantee page | Needed |