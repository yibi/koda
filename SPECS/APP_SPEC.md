# Etch — App Specification

## Overview

A journaling sanctuary for Gen Z. Entries save as plain `.md` files to the user's own iCloud Drive. No backend. No database. No subscription. No tracking. No streaks. No gratitude prompts.

The app is a **sanctuary** — a private space for decompression, not a tool for archiving. Every design decision serves one question: *does this help a 19-year-old process their day at 2am?*

## Core Philosophy

- **Sanctuary, not archive** — Built for today, not for 20 years from now.
- **Files are plain markdown** — If the app disappears, the files survive. The user owns everything.
- **No tracking** — No analytics, no telemetry, no accounts. We cannot see your entries.
- **No guilt mechanics** — No streaks, no reminders that feel like homework, no "you haven't journaled in 3 days!"
- **No toxic positivity** — Raw honesty. You can feel terrible. The app holds space for that.
- **Mood over text** — Sometimes you can't articulate how you feel. Etch lets you express mood without words.

---

## The 12 Features

### Phase 1 — MVP

---

### 1. The Threshold

**What it is:** A hold-to-enter opening screen. When you open Etch, you see a dark screen with a subtle prompt. You hold your thumb on the screen for ~1.5 seconds. The screen responds with haptic feedback and transitions into the sanctuary.

**Why it exists:** Gen Z needs a psychological boundary between the noise (TikTok, messages, notifications) and the sanctuary. A tap is too casual — you can tap through anything on autopilot. Holding requires intention. It's a ritual of entry, like taking off your shoes before entering a temple.

**How it works (tech):**
- Full-screen SwiftUI view with a gradient or solid dark background
- A circular hold area (or full-screen hold) detected via `DragGesture.MinimumDuration` or `LongPressGesture(minimumDuration: 1.5)`
- `UIImpactFeedbackGenerator` — haptic on start, stronger haptic on completion
- Transition: the dark screen "dissolves" or "opens" into the Canvas (opacity + scale animation)
- If Face ID/passcode is enabled, authentication happens during the hold (seamless)

**What it is NOT:**
- NOT a loading screen. The app is ready instantly. The hold is deliberate friction.
- NOT a login. There are no accounts. It's a psychological transition.
- NOT skippable. The hold IS the feature. Removing it kills the sanctuary feeling.

---

### 2. The Canvas

**What it is:** A free-form writing surface. When you enter the sanctuary, you see a blank canvas. Tap anywhere to place a text cursor and start writing. No "New Entry" button. No title field. No "Dear Diary." Just space.

**Why it exists:** Gen Z journaling is modular and raw — brain dumps, vibe checks, three lines about a feeling. Traditional journal apps force structure: title, date, body paragraph. That structure is a millennial frame. Gen Z needs a surface that accepts whatever they give it — a sentence, a word, a paragraph, nothing.

**How it works (tech):**
- SwiftUI `ZStack` with a background color/texture
- `TextEditor` or custom text view positioned at tap location
- Tap gesture on the canvas places a text input at that point
- Multiple text blocks can coexist on the same canvas (positioned freely)
- Entry saved as `.md` file with text content + metadata (vibe, date, position data in frontmatter)
- Auto-save on app backgrounding or after 3 seconds of inactivity

**What it is NOT:**
- NOT a lined notebook. No lines, no margins, no forced top-to-bottom writing.
- NOT a blank canvas with a toolbar. No formatting buttons floating around. Markdown works inline.
- NOT a word processor. No font picker, no colors, no "Insert Table." This is for feelings, not documents.

---

### 3. The Vibe Slider

**What it is:** A horizontal slider that sets the emotional temperature of an entry. Four positions: **Chaotic → Static → Melancholy → Glowing**. The slider changes the ambient color of the canvas as you move it.

**Why it exists:** Gen Z communicates mood through aesthetics, not words. "How are you?" is an impossible question at 2am. But sliding to "Chaotic" and watching the canvas turn deep red — that's an answer. The vibe slider lets you set the emotional atmosphere before you write a single word. If you can't articulate it, you can at least color it.

**How it works (tech):**
- SwiftUI `Slider` mapped to 4 discrete positions (0-3)
- Each position has a color gradient: Chaotic = deep red/black, Static = gray/blue, Melancholy = muted purple/navy, Glowing = warm amber/soft gold
- Canvas background animates to the selected gradient
- Vibe stored in entry frontmatter: `vibe: chaotic` (or `static`, `melancholy`, `glowing`)
- Haptic feedback on each position change (soft tick)

**What it is NOT:**
- NOT emoji selection. Emoji are performative. Vibe is atmospheric.
- NOT a 1-10 mood scale. Numbers imply measurement. Vibes imply feeling.
- NOT optional metadata that gets hidden. The vibe IS the canvas. It's the first thing you see when you reopen an entry.

---

### 4. The Wax Seal

**What it is:** When you finish an entry and it saves, a wax seal animation closes over it. A circle of warm color pools, solidifies, and stamps shut with a haptic click. The entry is sealed.

**Why it exists:** Saving in most apps feels like nothing — a spinner, a checkmark, a "Saved!" toast. That's a database operation, not an emotional act. Gen Z needs the act of saving to feel like closure. You poured something out. Now it's sealed. It's done. You can let it go. The wax seal turns "save" into a ritual of release.

**How it works (tech):**
- SwiftUI overlay animation on save trigger
- Wax pool: expanding circle with warm color (amber/red), opacity 0 → 0.8
- Solidify: circle edges sharpen, color deepens
- Stamp: scale bounce + `UIImpactFeedbackGenerator(.heavy)` — one firm haptic
- Fade out after 1 second, returning to entry list or canvas
- Duration: ~2 seconds total. Not skippable (but user can tap to dismiss after stamp)

**What it is NOT:**
- NOT a loading indicator. The save is instant. The animation is the emotional closure.
- NOT a confirmation dialog. No "Are you sure?" No "Save as draft?"
- NOT a lock. The entry can be reopened and edited. The seal is symbolic, not functional.

---

### 5. The Local Pulse

**What it is:** A small, steady green heartbeat indicator visible somewhere on the canvas (corner or edge). It pulses softly — a visual reminder that entries never leave the device. No server. No cloud. No company watching.

**Why it exists:** Gen Z has been surveilled their entire lives. Every app tracks them. Every platform monetizes their data. The Local Pulse is a constant, quiet reassurance: *this space is different. No one is watching. Your thoughts are yours.* It's not a feature you use — it's a feeling you trust.

**How it works (tech):**
- Small SwiftUI circle (8pt) with green color (#00C853 or similar)
- Pulsing animation: scale 1.0 → 1.3 → 1.0, opacity 0.6 → 1.0 → 0.6, 2-second cycle, `repeatForever`
- Positioned in a non-intrusive corner (bottom-right or status area)
- Tooltip on tap: "Your entries never leave this device."
- In settings: option to hide (some users may find it distracting)

**What it is NOT:**
- NOT a sync status indicator. There is no sync to show. It's a privacy heartbeat.
- NOT a notification badge. It doesn't count anything.
- NOT a logo or branding element. It's a reassurance signal.

---

### Phase 2 — Hooks

---

### 6. Internal Weather

**What it is:** An alternative to the Vibe Slider for when you can't even name what you're feeling. You paint your mood with touch. Drag your finger across the screen and colors bloom where you touch — stormy grays, anxious reds, calm blues, numb whites. Haptic feedback varies with the intensity of your stroke.

**Why it exists:** Sometimes words fail and even the 4-option Vibe Slider is too precise. Internal Weather is pre-verbal mood expression. It's the journaling equivalent of screaming into a pillow. You feel something intense and inarticulate, and you paint it out of your body and onto the screen. The haptics ground you in your body (somatic grounding technique from therapy).

**How it works (tech):**
- SwiftUI canvas with touch tracking via `DragGesture`
- Particle system or blob rendering at touch points — color depends on pressure/speed
- `UIImpactFeedbackGenerator` with intensity mapped to touch force (light touch = soft haptic, heavy press = strong haptic)
- Colors map to emotional ranges: slow/gentle = cool blues, fast/aggressive = reds, slow/heavy = dark purples, light/scattered = whites
- Result saved as an image alongside the .md entry (or as metadata in frontmatter)
- Can be used alone (no text) or alongside a text entry

**What it is NOT:**
- NOT a drawing tool. No brushes, no colors palette, no undo. It's raw expression.
- NOT saved as a polished image. It's a snapshot of a feeling, not an artwork.
- NOT shared or shareable. This is pure private catharsis.

---

### 7. The Echo

**What it is:** Once a day, when you enter the sanctuary, one past entry surfaces. Not "On This Day 2 years ago" (that's nostalgia — millennial). Instead, The Echo finds a past entry whose vibe resonates with your current state. It shows one line from it — a whisper, not a wall of text.

**Why it exists:** Gen Z journals to survive today, not to remember yesterday. But sometimes a past entry connects to the present in a way that helps: *"I felt this before and I'm still here."* That's not nostalgia — that's resonance. The Echo surfaces one connection, once a day, and then disappears. It doesn't drown you in your past.

**How it works (tech):**
- On app open (once per day max), query past entries
- Match by vibe similarity (same vibe slider position) or by text keyword overlap
- Display: one sentence from the matched entry, overlaid on the Threshold or Canvas
- User can tap to read the full entry, or swipe to dismiss
- After dismissal or reading, no more echoes until tomorrow
- Settings: can be disabled entirely

**What it is NOT:**
- NOT "On This Day" — not date-based. Vibe/resonance-based.
- NOT a feed of old entries. One entry. One whisper. Once a day.
- NOT nostalgia. The framing is "this still matters," not "remember when?"
- NOT a streak or habit mechanic. It doesn't count anything.

---

### 8. Anti-Journal Prompts

**What it is:** Optional writing prompts that appear only if you tap a small icon on the Canvas. The prompts are shadow work questions — real, uncomfortable, honest. Not "3 things you're grateful for." Things like: *"What did you pretend not to feel today?"* or *"Who did you perform for today, and why?"*

**Why it exists:** Gen Z is deep into shadow work (#ShadowWork: 2 billion TikTok views). They want to go deeper than surface-level journaling. But they hate boomer gratitude prompts and corporate wellness language. Anti-Journal Prompts give them real questions — the kind a therapist would ask, not an HR department. Off by default. You have to seek them out.

**How it works (tech):**
- Small icon (e.g., a subtle "?" or crescent moon) on the Canvas edge
- Tap reveals a random prompt from a curated list (stored locally in a JSON/plist)
- Prompt appears as faint text overlay on the canvas — user can write in response or dismiss
- ~50 prompts at launch, categorized: shadow work, anxiety processing, identity, relationships, existential
- No streak tracking. No "you completed 7 prompts!" gamification.
- Settings: customize prompt categories, disable entirely

**What it is NOT:**
- NOT gratitude prompts. Never. We don't do "3 good things."
- NOT forced. No popups. No "It's time to journal!" No notifications.
- NOT AI-generated. All prompts are human-written, curated, and vetted for tone.
- NOT daily. No "prompt of the day." You seek them when you need them.

---

### 9. The Burn Folder

**What it is:** A separate space in the app for entries that self-delete after 24 hours. You write something raw — anger, jealousy, shame, a text you'll never send — and instead of sealing it, you burn it. The entry exists for 24 hours (you can re-read it during that time), then it's permanently deleted. No trace.

**Why it exists:** Some thoughts need to be expressed but not kept. Therapy calls this "containment" — you put the feeling somewhere safe, then let it go. Traditional journal apps keep everything forever, which means every angry rant, every shameful thought, every 2am spiral lives permanently in your archive. The Burn Folder is for catharsis without permanence. You said it. It's out of your body. Now it burns.

**How it works (tech):**
- Separate folder/tab in the app: "Burn" (with a distinct visual style — darker, warmer tones)
- Entries written here have a 24-hour TTL (time-to-live) timestamp
- A visible countdown indicator on each burn entry
- At TTL expiry, the .md file is permanently deleted (`FileManager.removeItem`)
- During the 24 hours, user can re-read, add to, or manually burn (delete immediately)
- Burn animation: entry catches fire (particle effect or simple opacity burn), haptic, gone
- Burn folder entries are NOT included in search, Echo, or Color Cloud

**What it is NOT:**
- NOT a draft folder. Drafts are things you'll finish. Burn entries are things you'll never need again.
- NOT recoverable. No trash bin. No undo. Burn means burn.
- NOT a feature for everyone. Some users will never use it. That's fine.

---

### 10. The Redact Tool

**What it is:** A tool within the entry editor that lets you permanently black out your own words. Select text, tap "Redact," and the text is replaced with a solid black bar. Permanently. The original text is gone from the file. You can't un-redact it.

**Why it exists:** Sometimes you need to write something to process it, but you don't want those words to exist in your journal forever. The act of writing was the therapy — the words don't need to survive it. Redacting is different from deleting the whole entry (you might want to keep the context but destroy the specific words). It's also different from the Burn Folder (you're keeping the entry, just killing part of it). Redact is surgical destruction.

**How it works (tech):**
- Text selection in the editor → "Redact" button appears in context menu
- Selected text replaced with `[REDACTED]` in the .md file (or a Unicode black block: ████)
- The original text is NOT stored anywhere — no metadata, no hidden backup
- Visual: black bar rendered over the text position (CSS-style `background: black; color: black`)
- Haptic on redact (firm, final)
- Confirmation? Minimal — a quick "Redact permanently?" with no undo option after

**What it is NOT:**
- NOT edit/undo. Redact is permanent. There is no un-redact.
- NOT strikethrough. Strikethrough shows what was there. Redact destroys it.
- NOT a security feature. It's an emotional release feature. The point is the act of destroying the words.

---

### Phase 3 — Virality

---

### 11. The Color Cloud

**What it is:** At the end of each month, Etch generates an abstract color field — a visual representation of your month's vibes. Each entry's Vibe Slider position and Internal Weather data contribute colors. The result is a unique, abstract artwork: your emotional month as a color cloud. No text, no entries, no private data — just colors.

**Why it exists:** Gen Z shares aesthetics, not diaries. They won't post their journal entries on TikTok, but they will post a beautiful abstract color field that represents "my October." The Color Cloud is inherently shareable but inherently private — it contains zero personal content. It's a vibe, not a confession. This is the virality engine.

**How it works (tech):**
- Aggregate all entries from the past month
- Map vibe values to colors (Chaotic = red, Static = gray-blue, Melancholy = purple, Glowing = amber)
- Map Internal Weather data to texture/intensity
- Render as an abstract gradient/particle field using SwiftUI or Metal
- Each day of the month = one segment of the cloud (31 segments max)
- User can view, save to photos, or share
- No private content is included. Purely color + shape from mood data.

**What it is NOT:**
- NOT a mood chart or graph. No axes, no labels, no data viz. It's art.
- NOT a summary of what you wrote. No text excerpts, no word clouds.
- NOT a personality test result. No "You are 67% melancholic!" It's a color field.

---

### 12. End of Month Aura

**What it is:** A 5-second animated video of your month's emotional journey. The Color Cloud, but animated — colors shifting, blending, pulsing over 5 seconds. Set to ambient sound (or silent). Designed specifically for TikTok/Reels sharing. No text, no entries, no private data.

**Why it exists:** The Color Cloud is a static image. The Aura is a video. TikTok rewards video. A 5-second abstract animation of "my October vibes" is exactly the kind of content Gen Z shares — aesthetic, emotional, mysterious, zero personal risk. The Aura is the single most important virality feature in the app.

**How it works (tech):**
- Reuse Color Cloud data but animate it: colors shift over time, particles drift
- Render to a 5-second video using `AVAssetWriter` + `CADisplayLink` or Metal
- 1080x1920 (vertical, TikTok format)
- Optional ambient audio track (soft synth tone, or silent)
- Save to Photos, share sheet with TikTok/Instagram/Shorts presets
- Watermark: subtle "Etch" text in corner (optional, can be disabled)

**What it is NOT:**
- NOT a slideshow of entries. No text, no dates, no content.
- NOT a music video. No song integration. Ambient or silent.
- NOT a data visualization. It's a vibe. An aura. Not a report.