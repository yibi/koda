# Etch

**The place the algorithm can't see.**

Etch is a journaling sanctuary for Gen Z. Not an archive. Not a productivity tool. A private space to decompress from the noise — where your thoughts exist as plain files on your own device, invisible to every platform, every advertiser, every algorithm.

## The Problem

Gen Z is the most anxious generation on record. 91% report psychological symptoms from stress. Their attention is shattered by 6-second loops. Their social lives are performative. Their data belongs to everyone except them.

Every journaling app on the App Store is built for millennials — legacy archives, "Dear Diary" long-form writing, streak guilt, gratitude prompts that feel like an HR wellness portal. None of them understand why a 19-year-old opens a journal at 2am.

And they all want $35/year forever. Miss a payment, they lock the doors on your own thoughts.

## The Solution

- **A sanctuary, not a tool** — Built for decompression, not archiving. For surviving today, not remembering 20 years from now.
- **Plain files** — Every entry is a `.md` file in your iCloud. The algorithm can't see it. No company can lock you out. No server can lose it.
- **No subscription** — $24.99 one-time. Your safe space doesn't have a monthly rent.
- **No backend** — No servers. No cloud. No analytics. No tracking. We literally cannot see your entries.
- **No streak guilt** — No "you broke your 47-day streak!" notifications. You're already burned out. We're not adding more labor.
- **No toxic positivity** — No "3 things you're grateful for" prompts. You can be grateful for coffee AND depressed about housing. Etch holds space for both.

## The Angle

> *"The algorithm sees everything. Your likes, your location, your messages, your purchases. Etch is the one place it can't look. Your thoughts live as plain files on your device. No server. No tracking. No company between you and your own mind."*

## Target Market

- **Primary:** Gen Z (15-25) who journal to decompress, not to archive
- **Platform:** iOS first (iPhone + iPad)
- **Language:** English → Japanese → German

## Pricing

| Tier | Price | What you get |
|------|-------|-------------|
| Free | $0 | Up to 50 entries, all core sanctuary features |
| Pro | $24.99 one-time | Unlimited entries, all features, all future updates |

No "lifetime bundle." No tiers. No upsells. Pay once, own forever.

## Competitive Positioning

| App | Price | The Problem |
|-----|-------|-------------|
| Day One | $35/year | Subscription hostage, corporate-owned, millennial-focused, streak guilt |
| Journey | $40/year | Same subscription trap, clunky UI |
| Stoic | $40/year | Too "app-y," forced positivity, mood quizzes feel corporate |
| Apple Journal | Free | Too basic, locked to Apple, no real privacy from the ecosystem |
| Diarium | ~$15 one-time | Best non-sub competitor but UI looks like 2012 |
| **Etch** | **$24.99 once** | **Built for Gen Z sanctuary, plain .md files, no subscription, no tracking** |

## Tech Stack

- **Platform:** iOS 17+ (iPhone and iPad)
- **Language:** Swift + SwiftUI
- **Storage:** User's iCloud Drive via `UIDocumentPicker` — entries saved as `.md` files
- **Sync:** Handled by iOS (iCloud Drive). We never touch it.
- **Backend:** None. There is no backend.
- **Monetization:** StoreKit 2 (one-time IAP)
- **Analytics:** None. We don't track users. We can't track users.
- **Dependencies:** `swift-markdown-ui`, `KeychainAccess`

## Repository Structure

```
ETCH_Feature_Specification.pdf   12-feature spec (detailed)
PROJECT_PLAN.md                  Full plan: timeline, revenue, launch
SPECS/
  APP_SPEC.md                    Feature specifications (12 features)
  TECHNICAL_DESIGN.md            Architecture decisions for AI coding agents
DESIGN/
  BRAND_GUIDE.md                 Visual identity, tone, colors, typography
  SCREEN_FLOW.md                 Screen-by-screen UX map
  SCREEN_SPECS/                  Individual screen specifications
MARKETING/
  GUERRILLA_TACTICS.md           Unconventional marketing tactics
  LAUNCH_PLAN.md                 Launch timeline and playbook
SOURCE/                          Swift source code
```

## License

Proprietary. Source code will be open-sourced if the project is ever abandoned (End of Life guarantee in ToS).