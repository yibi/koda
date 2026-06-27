# Etch — Screen Flow & UX Map

## App Flow Overview

```
                    ┌──────────────┐
                    │  App Launch   │
                    └──────┬───────┘
                           │
                    ┌──────▼───────┐
                    │ Workspace     │── No workspace ──► ┌──────────────┐
                    │ bookmark      │    set             │  Onboarding  │
                    │ exists?       │◄───────────────────│  (Empty State)│
                    └──────┬───────┘                    └──────────────┘
                           │ Yes
                    ┌──────▼───────┐
                    │  THE THRESHOLD │  (hold to enter)
                    │  1.5s hold     │
                    └──────┬───────┘
                           │ Hold complete
                    ┌──────▼───────┐
                    │   THE CANVAS   │  (default view)
                    │   (free-form   │
                    │    writing)    │
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
       ┌──────▼──────┐  ┌──▼──────┐  ┌──▼──────────┐
       │ Entry List   │  │ Vibe    │  │ Burn Folder │
       │ (past entries│  │ Slider  │  │ (24h TTL    │
       │  grouped by  │  │ (mood)  │  │  entries)   │
       │  vibe/date)  │  └─────────┘  └──────┬──────┘
       └──────┬──────┘                      │
              │ Tap entry                    │ Write burn entry
              ▼                              ▼
       ┌──────────────┐              ┌──────────────┐
       │ Entry Detail │              │ Burn Canvas  │
       │ (read view)  │              │ (write +     │
       │              │              │  countdown)  │
       └──────┬───────┘              └──────┬───────┘
              │                              │
              │ Edit                         │ Save
              ▼                              ▼
       ┌──────────────┐              ┌──────────────┐
       │ Canvas Edit  │              │  WAX SEAL    │
       │ (+ Redact    │              │  (save anim) │
       │  Tool)       │              └──────────────┘
       └──────┬───────┘
              │ Save
              ▼
       ┌──────────────┐
       │  WAX SEAL    │
       │  (save anim) │
       └──────────────┘

  Overlay features (available on Canvas):
  ┌─────────────────────────────────────────────┐
  │  THE ECHO          — once/day, past whisper  │
  │  ANTI-JOURNAL      — shadow work prompts     │
  │  INTERNAL WEATHER  — touch-paint mood        │
  │  LOCAL PULSE       — green privacy heartbeat │
  └─────────────────────────────────────────────┘

  Monthly features (accessible from Entry List):
  ┌─────────────────────────────────────────────┐
  │  COLOR CLOUD     — monthly mood as art       │
  │  END OF MONTH AURA — 5s shareable video      │
  └─────────────────────────────────────────────┘
```

---

## Screen-by-Screen Specs

### SCREEN 1: Onboarding — First Launch

```
┌─────────────────────────────┐
│                             │
│                             │
│          [ etch ]            │
│                             │
│   the algorithm can't       │
│       see this.             │
│                             │
│  ──────────────────────     │
│                             │
│  Choose a folder for        │
│  your journal.              │
│  (iCloud Drive or local)    │
│                             │
│    ┌──────────────────┐     │
│    │  Choose Folder   │     │
│    └──────────────────┘     │
│                             │
│  Your entries are plain     │
│  .md files. No server.      │
│  No tracking. Nobody        │
│  watching.                  │
│                             │
└─────────────────────────────┘

Background: #000000 (OLED black)
Text: #E8E8E8 (off-white)
Brand text: #D4A574 (warm amber)
Button: amber border, transparent fill
Font: SF Pro Display Light for "etch", SF Pro Text for body
Film grain: 2-3% opacity overlay
```

### SCREEN 2: The Threshold — Hold to Enter

```
┌─────────────────────────────┐
│                             │
│                             │
│                             │
│                             │
│                             │
│                             │
│         ┌─────────┐         │
│         │  ◯      │         │
│         │ (hold)  │         │
│         └─────────┘         │
│                             │
│         hold to enter        │
│                             │
│                             │
│                             │
│                  ◉ (pulse)  │
└─────────────────────────────┘

Background: #000000
Hold circle: #666666 outline, fills with #D4A574 as held
Hint text: #E8E8E8 at 40% opacity, fades as hold progresses
Local Pulse: bottom-right corner, green #00C853, pulsing
Haptics: light on hold start, heavy on completion
Animation: circle fills clockwise over 1.5s, then screen dissolves to Canvas
Film grain: visible during hold (the "surface" you're pressing into)
```

### SCREEN 3: The Canvas — Writing Surface

```
┌─────────────────────────────┐
│  Mon Jun 28  ◉              │  ← date (SF Mono, gray) + Local Pulse
│                             │
│  ─────────────────────      │  ← vibe color gradient fills canvas
│                             │
│  |                          │  ← blinking cursor (tap anywhere)
│                             │
│                             │
│                             │
│  ─────────────────────      │
│  ◄ chaotic  static  ►       │  ← Vibe Slider (bottom)
│                  🔥  ☾  ◉   │  ← Burn | Prompts | Pulse icons
└─────────────────────────────┘

Background: Vibe-dependent gradient (Chaotic = deep red/black, etc.)
Text: #E8E8E8 (off-white)
Vibe Slider: Bottom bar, 4 positions, haptic tick on change
Icons: Burn (🔥), Anti-Journal Prompts (☾), Local Pulse (◉) — subtle, bottom-right
No "New Entry" button. No title field. No toolbar. Just space.
Tap anywhere on canvas → cursor appears → type.
Auto-save on background or 3s inactivity → triggers Wax Seal.
```

### SCREEN 4: The Wax Seal — Save Animation

```
┌─────────────────────────────┐
│                             │
│                             │
│         ┌─────────┐         │
│         │ ████████│         │  ← warm amber circle pools
│         │ ████████│         │    and solidifies
│         │ ████████│         │
│         └─────────┘         │
│                             │
│      [ entry sealed ]       │  ← subtle text, fades
│                             │
└─────────────────────────────┘

Phase 1 (0.5s): Amber circle expands from center, opacity 0 → 0.7
Phase 2 (0.5s): Circle solidifies, color deepens
Phase 3 (0.3s): Scale bounce (1.0 → 1.1 → 1.0) + heavy haptic
Phase 4 (0.7s): Fade to entry list or canvas
Total: ~2.0 seconds. Tap to dismiss after stamp phase.
```

### SCREEN 5: Entry List — Past Entries

```
┌─────────────────────────────┐
│  ◄ etch                     │
│                             │
│  This Month                 │
│  ┌─────────────────────────┐│
│  │ ●  Jun 28  2:14am       ││  ← vibe color dot
│  │    I can't sleep again.. ││    + first line preview
│  │    chaotic               ││    + vibe label
│  └─────────────────────────┘│
│  ┌─────────────────────────┐│
│  │ ●  Jun 27  11:30pm      ││
│  │    today was a lot       ││
│  │    melancholy            ││
│  └─────────────────────────┘│
│  ┌─────────────────────────┐│
│  │ ●  Jun 26  9:00pm       ││
│  │    actually good day?    ││
│  │    glowing               ││
│  └─────────────────────────┘│
│                             │
│  ┌─────────────────────────┐│
│  │  📊  Color Cloud         ││  ← monthly mood art
│  │  📹  End of Month Aura   ││  ← shareable video
│  └─────────────────────────┘│
│                             │
│  ─────────────────────      │
│  + new entry (canvas)       │  ← returns to Canvas
│  🔥 burn folder             │
│  ⚙  settings                │
└─────────────────────────────┘

Background: #000000
Cards: #0A0A0A surface, subtle border
Vibe dot: colored per vibe (red/gray/purple/amber)
Text: #E8E8E8 primary, #666666 secondary
No streak counter. No "you journaled 3 days in a row!" No guilt.
Entries sorted by date (most recent first) or grouped by vibe (toggle in settings).
```

### SCREEN 6: Entry Detail — Reading a Past Entry

```
┌─────────────────────────────┐
│  ◄ back        edit         │
│                             │
│  Mon Jun 28, 2:14am         │
│  ● chaotic                  │
│                             │
│  ─────────────────────      │
│                             │
│  I can't sleep again.       │
│  My brain won't shut up     │
│  about tomorrow.            │
│                             │
│  I keep thinking about      │
│  what she said and I know   │
│  I shouldn't care but ████  │  ← redacted text (black bars)
│  and it's eating me alive.  │
│                             │
│  ─────────────────────      │
│                  ◉ (pulse)  │
└─────────────────────────────┘

Background: Vibe gradient (same as when entry was written)
Text: #E8E8E8
Redacted text: Solid black bars (#000000) over text position
Edit button: Opens Canvas with entry content loaded
Local Pulse: bottom-right, green, pulsing
```

### SCREEN 7: Internal Weather — Touch Painting

```
┌─────────────────────────────┐
│  ◄ back                     │
│                             │
│  paint how you feel         │
│                             │
│  ─────────────────────      │
│                             │
│     ·    ··                 │
│   ·██████·                  │  ← touch trails leave color
│    ·████··                  │    and texture
│     ····                    │
│                             │
│                             │
│  ─────────────────────      │
│  (touch anywhere.           │
│   there are no wrong        │
│   colors.)                  │
│                  ◉ (pulse)  │
└─────────────────────────────┘

Background: #000000 (starts blank)
Touch trails: Color depends on pressure/speed
  - Slow/gentle: cool blues
  - Fast/aggressive: reds
  - Slow/heavy: dark purples
  - Light/scattered: whites
Haptics: Intensity mapped to touch force
Save: Snapshot stored alongside entry. Can be used without text.
No brush picker. No color palette. No undo. Raw expression only.
```

### SCREEN 8: The Echo — Past Entry Whisper

```
┌─────────────────────────────┐
│                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─     │
│                             │
│  "I felt this before        │
│   and I'm still here."      │
│                             │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─     │
│                             │
│  ┌─────────────────────────┐│
│  │ Mar 14  ·  melancholy   ││
│  │                         ││
│  │ "some days the weight   ││  ← one sentence from
│  │  is just... heavy."     ││    a past entry
│  │                         ││
│  │  tap to read            ││
│  └─────────────────────────┘│
│                             │
│         swipe to dismiss     │
│                             │
└─────────────────────────────┘

Appears as overlay on Threshold or Canvas (once per day max).
Background: semi-transparent black over canvas
Card: #0A0A0A with subtle border, faded vibe gradient
Text: #E8E8E8, italic for the whisper
One entry. One sentence. Once a day. Then gone.
Not "On This Day." Vibe-resonance matched.
```

### SCREEN 9: Anti-Journal Prompts

```
┌─────────────────────────────┐
│                             │
│                             │
│                             │
│  What did you pretend       │
│  not to feel today?         │
│                             │
│                             │
│                             │
│  ─────────────────────      │
│  write  ·  another  ·  ✕    │
│                             │
└─────────────────────────────┘

Background: #000000 with subtle film grain
Prompt text: #E8E8E8, SF Pro Display Light, centered, large
Buttons: text-only, gray, minimal
Tap "write" → prompt stays as faint overlay on Canvas, user writes below
Tap "another" → new random prompt from same category
Tap "✕" → dismiss, return to Canvas
No streak tracking. No "you completed 7 prompts!" No gamification.
```

### SCREEN 10: The Burn Folder

```
┌─────────────────────────────┐
│  ◄ back                     │
│                             │
│  burn                       │
│  ─────────────────────      │
│                             │
│  ┌─────────────────────────┐│
│  │ ●  burns in 18:42:31    ││  ← countdown timer
│  │    I'm so angry at      ││
│  │    them right now...    ││
│  │    chaotic              ││
│  └─────────────────────────┘│
│  ┌─────────────────────────┐│
│  │ ●  burns in 03:15:00    ││
│  │    the text I'll never  ││
│  │    send...              ││
│  │    melancholy           ││
│  └─────────────────────────┘│
│                             │
│  ─────────────────────      │
│  + new burn entry           │
│                             │
│  these entries self-delete  │
│  in 24 hours. permanently.  │
│  no recovery. no trace.     │
└─────────────────────────────┘

Background: #000000 (slightly warmer/darker than main canvas)
Cards: #0A0A0A with warm amber border accent
Countdown: SF Mono, amber color
Vibe dot: same colors as main entries
Warning text: #666666, small, at bottom
Burn entries are NOT in search. NOT in Echo. NOT in Color Cloud.
```

### SCREEN 11: The Redact Tool — In Editor

```
┌─────────────────────────────┐
│  ◄ back                     │
│                             │
│  I'm so tired of [██████]   │  ← selected text
│  and I can't take it          │    highlighted
│  anymore.                     │
│                             │
│  ─────────────────────      │
│  ┌───────────────────────┐  │
│  │  Redact permanently?  │  │  ← minimal confirmation
│  │                       │  │
│  │  This cannot be       │  │
│  │  undone.              │  │
│  │                       │  │
│  │  [Redact]  [Cancel]   │  │
│  └───────────────────────┘  │
│                             │
└─────────────────────────────┘

Redact replaces selected text with █ (U+2588) in the .md file.
Original text is NOT stored anywhere. Permanent.
Haptic: heavy impact on redact confirmation.
```

### SCREEN 12: The Color Cloud — Monthly Mood Art

```
┌─────────────────────────────┐
│  ◄ back                     │
│                             │
│  June 2026                  │
│                             │
│  ┌─────────────────────────┐│
│  │                         ││
│  │   ░ ▒ ▓ █ ▒ ░ ▒ ▓ ░    ││  ← abstract color field
│  │    ▒ █ ▓ ░ ▒ █ ▓ ▒     ││    generated from
│  │  ░ ▓ █ ▒ ░ ▒ ▓ █ ░     ││    monthly vibe data
│  │   █ ▒ ░ ▓ █ ▒ ░ ▓      ││
│  │  ░ ▒ ▓ █ ▒ ░ ▒ ▓ ░    ││
│  │                         ││
│  └─────────────────────────┘│
│                             │
│  your month in color.       │
│  no words. no entries.      │
│  just vibes.                │
│                             │
│  ┌────────┐  ┌───────────┐  │
│  │  Save  │  │  Share    │  │
│  └────────┘  └───────────┘  │
│                             │
│  ─────────────────────      │
│  📹 Generate End of Month   │  ← link to Aura video
│    Aura video               │
└─────────────────────────────┘

Background: #000000
Color cloud: SwiftUI Canvas or Metal, abstract gradient/particle field
Each day = one segment, colored by that day's vibe(s)
Save: UIImageWriteToSavedPhotosAlbum
Share: UIActivityViewController (Instagram, TikTok, etc.)
No text, no entries, no private data. Purely color from mood.
```

### SCREEN 13: End of Month Aura — Shareable Video

```
┌─────────────────────────────┐
│                             │
│  ┌─────────────────────────┐│
│  │                         ││
│  │   [animated color       ││  ← 5-second video
│  │    field, colors        ││    vertical 1080x1920
│  │    shifting and         ││
│  │    blending over time]  ││
│  │                         ││
│  │                         ││
│  │                         ││
│  │           etch          ││  ← subtle watermark
│  └─────────────────────────┘│
│                             │
│  your month in motion.      │
│  5 seconds. no words.       │
│                             │
│  ┌────────┐  ┌───────────┐  │
│  │  Save  │  │  Share to  │  │
│  └────────┘  │  TikTok    │  │
│              └───────────┘  │
└─────────────────────────────┘

Video: 5 seconds, 1080x1920 (vertical), 30fps
Content: Animated Color Cloud data — colors shift, blend, pulse
Audio: Silent or optional ambient tone
Watermark: "etch" in corner (disable in settings)
Export: Save to Photos or share directly to TikTok/Instagram/Shorts
Zero private data. No text. No entries. Just colors in motion.
```

### SCREEN 14: Settings

```
┌─────────────────────────────┐
│  ◄ back                     │
│                             │
│  settings                   │
│  ─────────────────────      │
│                             │
│  Sanctuary                  │
│  ┌─────────────────────────┐│
│  │ Haptics          [ on ] ││
│  │ Local Pulse      [ on ] ││
│  │ The Echo         [ on ] ││
│  └─────────────────────────┘│
│                             │
│  Prompts                    │
│  ┌─────────────────────────┐│
│  │ Shadow Work      [ on ] ││
│  │ Anxiety          [ on ] ││
│  │ Identity         [ on ] ││
│  │ Relationships   [off ] ││
│  │ Existential     [off ] ││
│  └─────────────────────────┘│
│                             │
│  Privacy                    │
│  ┌─────────────────────────┐│
│  │ App Lock (Face ID)[off ]││
│  │ Aura Watermark   [ on ] ││
│  └─────────────────────────┘│
│                             │
│  About                      │
│  ┌─────────────────────────┐│
│  │ End of Life Guarantee   ││
│  │ Privacy Policy          ││
│  │ Version 1.0             ││
│  └─────────────────────────┘│
│                             │
│  ─────────────────────      │
│  ◉ your entries never       │
│    leave this device        │
└─────────────────────────────┘

Background: #000000
Sections: SF Pro Display Light, gray headers
Toggles: System toggle style, amber accent when on
No "upgrade to pro" buttons. No ads. No cross-promotion.
End of Life Guarantee: "If we abandon this app, we open-source the code."
```

---

## Navigation Summary

| From | To | How |
|------|-----|-----|
| Launch (no workspace) | Onboarding | Automatic |
| Launch (workspace set) | Threshold | Automatic |
| Threshold (hold complete) | Canvas | Automatic (dissolve transition) |
| Canvas | Entry List | Swipe or tap list icon |
| Canvas | Burn Folder | Tap burn icon |
| Canvas | Internal Weather | Tap weather icon (or gesture) |
| Canvas | Anti-Journal Prompt | Tap prompt icon |
| Canvas (save) | Wax Seal → Entry List | Automatic |
| Entry List | Entry Detail | Tap entry card |
| Entry Detail | Canvas (edit mode) | Tap "edit" |
| Entry List | Color Cloud | Tap "Color Cloud" |
| Color Cloud | End of Month Aura | Tap "Generate Aura" |
| Any screen | Settings | Gear icon (in Entry List) |
| Any screen | Canvas | "+ new entry" or back to Canvas |