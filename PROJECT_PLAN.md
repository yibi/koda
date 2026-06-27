# Etch — Complete Project Plan

## Executive Summary

**Product:** A journaling sanctuary for Gen Z. Entries save as plain `.md` files to user's iCloud Drive. No backend. No subscription. No tracking. No streaks. No corporate wellness vibes.

**Brand:** Etch — "The place the algorithm can't see."

**Core angle:** Gen Z journals to survive, not to archive. Etch is a sanctuary for decompression, not a tool for legacy.

**Target:** $30,000 revenue Year 1 (1,200 sales at $24.99 one-time IAP)

**Developer:** Solo product manager. AI coding agents write all Swift code. User reviews and ships.

**Machine:** MacBook (already have). $0 hardware cost.

**Timeline:** 8-12 weeks (AI agents code, user reviews)

**Investment:** ~$150 (Apple Developer account + app icon)

---

## 1. The Opportunity

### Validated Demand

| Metric | Data |
|--------|------|
| Day One monthly revenue | ~$600k ($7.2M/year) |
| Day One 1-star reviews mentioning subscription | 45% |
| #Journaling on TikTok | 4.5 billion views |
| #ShadowWork on TikTok | 2 billion views |
| Gen Z stress/anxiety rate | 91% report psychological symptoms (APA) |
| Journaling search trend | Growing 22% YoY |
| Gap | No journaling app built for Gen Z sanctuary — all are millennial archive tools |

### Why Gen Z Will Switch

1. **No app understands them** — Every journaling app is built for millennials (legacy, long-form, gratitude prompts). Gen Z wants modular, raw, mood-first, private.
2. **Subscription fatigue** — Gen Z is already paying for 5+ subscriptions. A journal shouldn't be another monthly bill.
3. **Privacy hunger** — The algorithm sees everything. Gen Z wants one space it can't see.
4. **Streak guilt is toxic** — Duolingo-style shaming for missing a day = more labor. Gen Z is already burned out.
5. **Corporate wellness is cringe** — If it looks like an HR portal or Slack, they're out.

### The USP

> *"The place the algorithm can't see."*

Not "no subscription" (that's pricing). Not "markdown files" (that's tech). The USP is **sanctuary** — a private space designed for decompression, invisible to every platform, owned entirely by the user.

---

## 2. Product Spec

### The 12 Features

Built in three phases. Each feature has a detailed spec in `SPECS/APP_SPEC.md`.

#### MVP (Phase 1 — Weeks 1-4)

| Feature | What It Does |
|---------|-------------|
| **The Threshold** | Hold-to-enter opening ritual. Not a login — a psychological transition into sanctuary space. |
| **The Canvas** | Free-form writing surface. Tap anywhere to write. No rigid format. No "Dear Diary." |
| **The Vibe Slider** | Set emotional temperature: Chaotic / Static / Melancholy / Glowing. Not emoji — mood as atmosphere. |
| **The Wax Seal** | Privacy animation on save. Not a spinner — a wax seal closes over your entry. Saving feels final, safe. |
| **The Local Pulse** | Constant green privacy heartbeat. Visual reminder that entries never leave the device. No server. No cloud. |

#### Phase 2 — Hooks (Weeks 5-8)

| Feature | What It Does |
|---------|-------------|
| **Internal Weather** | Paint your mood with touch and haptics. Not a dropdown — a visceral, tactile mood capture. |
| **The Echo** | One whisper from the past. Not nostalgia ("remember this?") — resonance ("this still matters"). Shows one past entry that connects to today's vibe. |
| **Anti-Journal Prompts** | Shadow work prompts. Off by default. No gratitude lists. Real questions: "What did you pretend not to feel today?" |
| **The Burn Folder** | Entries that self-delete in 24 hours. For thoughts you need to express but don't need to keep. Digital catharsis. |
| **The Redact Tool** | Permanently black out your own words. Some things need to be said then destroyed. Not edit — redact. |

#### Phase 3 — Virality (Weeks 9-12)

| Feature | What It Does |
|---------|-------------|
| **The Color Cloud** | Monthly mood visualized as abstract art. Your emotional month as a color field. Shareable, but contains no private content. |
| **End of Month Aura** | 5-second TikTok-shareable video. Abstract animation of your month's vibes. No text, no entries, no private data — just colors and motion. |

### Cut Features (Not Building)

| Feature | Why Cut |
|---------|---------|
| Ghost Mode | Over-engineered. Adds complexity without serving the core sanctuary need. |
| PDF book export | Millennial legacy feature. Gen Z doesn't want to print a book of their journal. |
| "On This Day" nostalgia | Nostalgia is a millennial need. Gen Z journals for today, not for looking back. |
| Streak tracking | Toxic. Adds guilt, not relief. The opposite of sanctuary. |
| Gratitude prompts | Toxic positivity. Boomer energy. Reductive. |
| Social sharing of entries | The whole point is privacy. Sharing entries defeats the purpose. |
| AI-powered insights | Feels corporate. "Your AI wellness coach" is exactly what Gen Z hates. |

---

## 3. Revenue Model

| | Value |
|---|---|
| Price | $24.99 one-time |
| Free tier | Up to 50 entries (all sanctuary features work) |
| Pro tier | Unlimited entries, all features, all future updates |
| Year 1 target | 1,200 sales = $30,000 |
| Apple's cut (30%) | $9,000 |
| Net Year 1 | $21,000 |
| Costs | ~$150 (Apple Developer account + icon) |

### Revenue Strategy

- **Gen Z drives virality** — Color Cloud and End of Month Aura are designed for TikTok sharing. They contain zero private data but communicate the "vibe" of the app.
- **Free tier is generous** — 50 entries lets users genuinely experience the sanctuary before paying. No "7-day trial" manipulation.
- **No dark patterns** — No "upgrade now!" popups mid-entry. No paywall on emotional features. The paywall is only on entry count.

---

## 4. Build Plan

### Phase 1: MVP (Weeks 1-4)

| Week | Deliverable |
|------|-------------|
| 1 | Project setup, file system layer (iCloud folder picker, .md read/write), Threshold screen |
| 2 | Canvas writing surface, Vibe Slider, entry model |
| 3 | Wax Seal save animation, Local Pulse indicator, entry list view |
| 4 | Settings, onboarding flow, polish, internal testing |

### Phase 2: Hooks (Weeks 5-8)

| Week | Deliverable |
|------|-------------|
| 5 | Internal Weather (touch + haptics mood painting) |
| 6 | The Echo (past entry resonance matching) |
| 7 | Anti-Journal Prompts, The Burn Folder (24-hour self-delete) |
| 8 | The Redact Tool, polish, TestFlight beta |

### Phase 3: Virality (Weeks 9-12)

| Week | Deliverable |
|------|-------------|
| 9 | The Color Cloud (monthly mood abstract art) |
| 10 | End of Month Aura (TikTok-shareable video generation) |
| 11 | App Store screenshots, listing, ASO |
| 12 | Launch |

---

## 5. Marketing Strategy

### Core Narrative

*"Every app wants your data. Your likes, your location, your messages, your purchases. Etch is the one app that physically cannot see your data. Your entries are plain files on your device. No server. No analytics. No algorithm. Just you and your thoughts."*

### Channels

| Channel | Strategy |
|---------|----------|
| TikTok | #Journaling (4.5B views), #ShadowWork (2B views), #MentalHealth. Post Color Cloud and Aura videos. ASMR journaling aesthetics. |
| Reddit | r/journaling, r/GenZ, r/mentalhealth, r/privacy. Not r/productivity. |
| Product Hunt | "Anti-launch" — "The journaling app that can't see your journal" |
| Hacker News | Privacy + local-first angle. No-subscription angle. |

### NOT Targeting

| Channel | Why |
|---------|-----|
| r/productivity | Wrong audience. This isn't productivity. |
| LinkedIn | Corporate network. Anti-sanctuary. |
| Tech reviewers | They review features. Etch sells a feeling. |
| Millennial journaling communities | They want archives and legacy. Wrong product. |

Full marketing plan: `MARKETING/LAUNCH_PLAN.md` and `MARKETING/GUERRILLA_TACTICS.md`.

---

## 6. Risks

| Risk | Mitigation |
|------|-----------|
| Gen Z won't pay $24.99 | Free tier is generous. Paywall is on count, not features. Virality drives downloads. |
| Apple rejects the app | Follow HIG. No dark patterns. Clear IAP disclosure. End of Life guarantee in ToS. |
| iCloud sync issues | iOS handles sync natively. We just read/write files. Test with real iCloud accounts. |
| Day One copies features | They can't copy the philosophy. Their $35/year model can't become "sanctuary." |
| App Store discovery is hard | TikTok virality > App Store search. Color Cloud and Aura are built for sharing. |