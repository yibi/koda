# Etch — Technical Design Document

*This document resolves every technical decision so any AI coding agent can build the app without ambiguity.*

---

## 1. Project Foundation

| Decision | Value | Rationale |
|----------|-------|-----------|
| Framework | **SwiftUI** | Modern, declarative, Apple's recommended path. AI agents write SwiftUI better than UIKit. |
| Minimum iOS | **iOS 17.0** | SwiftUI is mature at 17. StoreKit 2 works. NavigationStack available. Don't support older — Gen Z has newer phones. |
| Bundle ID | **com.yibi.etch** | Clean, brandable. |
| App type | **Library-based app** (not Document-based) | User picks a workspace folder once. App manages .md files inside it. Not a document browser — that's Apple's job. |
| Dependency manager | **Swift Package Manager (SPM)** | Apple-native, no CocoaPods, no Carthage. |
| Device target | **iPhone only (MVP)** | Universal app later. iPhone first because Gen Z is on iPhone. iPad layout is V2. |
| Orientation | **Portrait only** | Journaling is a vertical activity. Simplifies layout. |
| iCloud | **Yes — user's own iCloud Drive** | App uses UIDocumentPicker to let user select any folder (including iCloud Drive). iOS handles sync. App never touches the network. |

---

## 2. Architecture

### 2.1 App Structure

```
Etch/
├── EtchApp.swift                 ← @main entry point
├── App/
│   ├── AppState.swift            ← Global state: workspace URL, IAP status, settings
│   └── Constants.swift           ← Colors, fonts, IAP product IDs, config keys
├── Models/
│   ├── Entry.swift               ← Entry model: filename, content, vibe, date, weather data
│   ├── EntryMetadata.swift       ← Vibe, weather canvas data, burn TTL, redact positions
│   └── Workspace.swift           ← Selected folder: bookmark URL, display name, entry count
├── Views/
│   ├── ThresholdView.swift       ← Hold-to-enter opening ritual
│   ├── CanvasView.swift          ← Free-form writing surface (tap anywhere to write)
│   ├── VibeSliderView.swift      ← Emotional temperature slider (4 positions)
│   ├── WaxSealView.swift         ← Save animation overlay (wax seal closing)
│   ├── LocalPulseView.swift      ← Green privacy heartbeat indicator
│   ├── EntryListView.swift       ← List of past entries (grouped by vibe or date)
│   ├── EntryDetailView.swift     ← Read view for a past entry
│   ├── InternalWeatherView.swift ← Touch-painting mood canvas with haptics
│   ├── EchoView.swift            ← One past entry whisper overlay
│   ├── AntiJournalPromptView.swift ← Shadow work prompt overlay
│   ├── BurnFolderView.swift      ← 24-hour self-delete entries
│   ├── RedactToolView.swift      ← Text redaction overlay in editor
│   ├── ColorCloudView.swift      ← Monthly mood abstract art
│   ├── EndOfMonthAuraView.swift  ← 5-second TikTok-shareable video generation
│   ├── SettingsView.swift        ← Sanctuary settings (minimal, no dark patterns)
│   ├── FolderPickerView.swift    ← UIDocumentPicker wrapper
│   └── EmptyStateView.swift      ← First-launch onboarding
├── Services/
│   ├── FileService.swift         ← All FileManager operations: list, read, write, delete
│   ├── BookmarkService.swift     ← Security-scoped bookmark persistence/resolution
│   ├── MarkdownService.swift     ← Parse markdown to attributed string
│   ├── MetadataService.swift     ← Read/write entry frontmatter (vibe, weather, burn TTL)
│   ├── HapticService.swift       ← Centralized haptic feedback manager
│   ├── BurnService.swift         ← 24-hour TTL management, burn animation, auto-delete
│   ├── EchoService.swift         ← Past entry resonance matching (vibe + keyword)
│   ├── PromptService.swift       ← Load and curate anti-journal prompts from local JSON
│   ├── ColorCloudService.swift   ← Aggregate monthly vibe data → color field
│   ├── AuraRenderer.swift        ← Render 5-second video from Color Cloud data
│   └── IAPService.swift          ← StoreKit 2 one-time purchase
├── Resources/
│   ├── Prompts.json              ← 50+ anti-journal prompts (shadow work, anxiety, identity)
│   ├── Colors.swift              ← Vibe color palettes (Chaotic, Static, Melancholy, Glowing)
│   └── Assets.xcassets           ← App icon, accent colors, textures
└── Info.plist
```

### 2.2 Entry File Format

Each entry is a `.md` file with YAML-like frontmatter:

```markdown
---
date: 2026-06-28T02:14:00
vibe: chaotic
weather: data:image/png;base64,...  (optional, if Internal Weather was used)
burn: false
burn_ttl: null
---

I can't sleep again. My brain won't shut up about tomorrow.
```

For burn entries:
```markdown
---
date: 2026-06-28T02:14:00
vibe: chaotic
burn: true
burn_ttl: 2026-06-29T02:14:00
---

[content that will self-delete in 24 hours]
```

For redacted text, the `.md` file contains:
```markdown
I'm so tired of ████████████ and I can't take it anymore.
```

The original text is NOT stored anywhere. `████` replaces the characters permanently.

### 2.3 State Management

```swift
@Observable final class AppState {
    var workspaceURL: URL?           // Security-scoped bookmark to user's folder
    var isPro: Bool                   // IAP status
    var entries: [Entry]              // In-memory list, loaded from files
    var burnEntries: [Entry]          // Separate list for burn folder
    var settings: Settings            // Haptics on/off, pulse visible, echo on/off, prompts category
    var hasSeenEchoToday: Bool        // Prevents multiple echoes per day
    var entryCount: Int               // For free tier limit (50)
}
```

No Core Data. No SQLite. The file system IS the database. Files are read into memory on launch and on directory changes (via `DispatchSource` file system monitoring).

### 2.4 File System Monitoring

```swift
// Watch the workspace folder for changes (iCloud sync may add/remove files)
let dispatchSource = DispatchSource.makeFileSystemObjectSource(
    fileDescriptor: fd,
    eventMask: .write,
    queue: .main
)
dispatchSource.setEventHandler { [weak self] in
    self?.refreshEntries()
}
```

This handles iCloud sync: when iOS syncs a new `.md` file from another device, the entry list updates automatically.

---

## 3. Feature Technical Details

### 3.1 The Threshold (Hold-to-Enter)

```swift
struct ThresholdView: View {
    @State private var isHolding = false
    @State private var progress: CGFloat = 0

    var body: some View {
        ZStack {
            Color.etchBlack.ignoresSafeArea()

            // Subtle hint text
            Text("Hold to enter")
                .font(.system(size: 14, weight: .light))
                .foregroundStyle(.white.opacity(0.4))
                .opacity(1 - progress)  // Fades as you hold

            // Progress circle
            Circle()
                .stroke(.white.opacity(0.15), lineWidth: 2)
                .frame(width: 60, height: 60)
                .overlay {
                    Circle()
                        .trim(from: 0, to: progress)
                        .stroke(Color.etchAccent, style: StrokeStyle(lineWidth: 2, lineCap: .round))
                        .rotationEffect(.degrees(-90))
                        .frame(width: 60, height: 60)
                }
        }
        .gesture(
            LongPressGesture(minimumDuration: 1.5)
                .onChanged { _ in
                    HapticService.shared.impact(.light)
                    withAnimation(.linear(duration: 1.5)) {
                        progress = 1.0
                    }
                }
                .onEnded { _ in
                    HapticService.shared.impact(.heavy)
                    withAnimation(.easeInOut(duration: 0.8)) {
                        // Transition to Canvas
                    }
                }
        )
    }
}
```

### 3.2 The Canvas (Free-Form Writing)

- `ZStack` with vibe-colored background
- Tap gesture creates a `TextEditor` at tap point
- Multiple text blocks stored as positioned elements
- On save, text blocks are concatenated into a single `.md` file (position data in frontmatter if needed for re-rendering)
- For MVP: single text block per entry (simpler). Multi-block is V2.

### 3.3 Vibe Slider

| Position | Name | Primary Color | Secondary Color |
|----------|------|---------------|-----------------|
| 0 | Chaotic | #8B0000 (deep red) | #1A0000 |
| 1 | Static | #4A5568 (steel gray) | #1A202C |
| 2 | Melancholy | #4A3F6B (muted purple) | #1A1530 |
| 3 | Glowing | #D4A574 (warm amber) | #2A2010 |

```swift
enum Vibe: Int, CaseIterable, Codable {
    case chaotic = 0
    case static_ = 1
    case melancholy = 2
    case glowing = 3

    var gradient: LinearGradient { ... }
    var label: String { ... }
}
```

### 3.4 Wax Seal Animation

1. **Pool phase** (0.5s): Warm amber circle expands from center, opacity 0 → 0.7
2. **Solidify phase** (0.5s): Circle edges sharpen, color deepens to dark amber
3. **Stamp phase** (0.3s): Scale bounce (1.0 → 1.1 → 1.0) + heavy haptic
4. **Fade phase** (0.7s): Opacity → 0, revealing entry list or canvas

Total: ~2.0 seconds. Tap-to-dismiss available after stamp phase.

### 3.5 Local Pulse

```swift
struct LocalPulseView: View {
    @State private var isPulsing = false

    var body: some View {
        Circle()
            .fill(Color.etchGreen)
            .frame(width: 8, height: 8)
            .scaleEffect(isPulsing ? 1.3 : 1.0)
            .opacity(isPulsing ? 1.0 : 0.6)
            .animation(.easeInOut(duration: 1.0).repeatForever(autoreverses: true), value: isPulsing)
            .onTapGesture {
                // Show tooltip: "Your entries never leave this device."
            }
            .onAppear { isPulsing = true }
    }
}
```

### 3.6 Internal Weather

- Canvas with `DragGesture` tracking touch points
- `UIImpactFeedbackGenerator` with `intensity` mapped to touch force
- Render: particle system using SwiftUI Canvas or Metal
- Save: render snapshot to `UIImage`, store as base64 in frontmatter or as separate `.png` file alongside `.md`

### 3.7 The Echo

- On app open, check `UserDefaults.lastEchoDate`
- If not today, query entries from >7 days ago
- Match by: same `vibe` value OR keyword overlap (simple token matching, no NLP)
- Display one sentence from matched entry (split by `. `, take first)
- Update `lastEchoDate` to today
- Settings toggle: `echoEnabled: Bool`

### 3.8 Anti-Journal Prompts

- `Prompts.json` bundled in app:
```json
[
  {
    "id": "shadow_01",
    "category": "shadow_work",
    "text": "What did you pretend not to feel today?"
  },
  {
    "id": "shadow_02",
    "category": "shadow_work",
    "text": "Who did you perform for today, and why?"
  },
  {
    "id": "anxiety_01",
    "category": "anxiety",
    "text": "What is the worst case scenario you're spinning on, and what would actually happen if it occurred?"
  }
]
```
- ~50 prompts at launch, human-written and curated
- Categories: `shadow_work`, `anxiety`, `identity`, `relationships`, `existential`
- User selects active categories in settings

### 3.9 The Burn Folder

- Separate subfolder in workspace: `/burn/` directory
- Burn entries have `burn: true` and `burn_ttl` in frontmatter
- Background timer checks every 60 seconds for expired TTLs
- On expiry: `FileManager.removeItem` — permanently deleted
- Burn animation: particle effect (spark + fade) or simple opacity burn with red/orange color
- Manual burn: user taps "Burn now" → immediate delete with animation

### 3.10 The Redact Tool

- In `CanvasView`, text selection via SwiftUI context menu
- "Redact" option replaces selected text with `█` (U+2588 FULL BLOCK) characters
- Original text is NOT stored — the replacement is in the `.md` file itself
- Haptic: `.heavy` impact
- Minimal confirmation: alert "Redact permanently? This cannot be undone." → [Redact] [Cancel]

### 3.11 Color Cloud

- Query all entries from current month
- For each entry: map `vibe` to a color, map `weather` data to texture intensity
- 31 segments (one per day), rendered as a gradient/particle field
- SwiftUI `Canvas` or Metal for rendering
- Export: `UIImageWriteToSavedPhotosAlbum`

### 3.12 End of Month Aura

- Reuse Color Cloud data, animate over 5 seconds
- `AVAssetWriter` with `AVAssetWriterInputPixelBufferAdaptor`
- 1080x1920 (vertical video for TikTok/Reels)
- 30fps, 150 frames total (5 seconds)
- Each frame: interpolate color cloud state
- Optional ambient audio: bundled short tone or silent
- Export to Photos, share via `UIActivityViewController`

---

## 4. Data Model

### Entry

```swift
struct Entry: Identifiable, Codable {
    let id: UUID
    let filename: String          // e.g., "2026-06-28-0214.md"
    let date: Date
    var content: String           // Markdown text content
    var vibe: Vibe                // .chaotic, .static_, .melancholy, .glowing
    var weatherImage: Data?       // Internal Weather snapshot (optional)
    var isBurn: Bool              // Is this in the Burn Folder?
    var burnTTL: Date?            // When to auto-delete (if burn)
    var isRedacted: Bool          // Does entry contain redacted text?

    // Computed
    var wordCount: Int { ... }
    var isExpired: Bool { ... }   // burnTTL < now
}
```

### Settings

```swift
struct Settings: Codable {
    var hapticsEnabled: Bool = true
    var localPulseVisible: Bool = true
    var echoEnabled: Bool = true
    var promptCategories: Set<String> = ["shadow_work", "anxiety", "identity"]
    var defaultVibe: Vibe = .static_
    var burnAutoDelete: Bool = true
    var auraWatermark: Bool = true
}
```

---

## 5. Security & Privacy

| Aspect | Implementation |
|--------|---------------|
| Data storage | Plain `.md` files in user-selected iCloud Drive folder |
| Encryption | iOS full-disk encryption (Data Protection class: `.complete`) |
| App lock | Face ID / passcode via `LocalAuthentication` (optional, off by default) |
| Network access | **None.** The app has no network code. No `URLSession`, no push, no analytics. |
| Analytics | **None.** Zero. No telemetry, no crash reporting that sends data. |
| Tracking | **None.** No ATT prompt needed. We don't track. |
| Data collection | **None.** We don't collect anything. There is no server to collect it. |

### Privacy Manifest (PrivacyInfo.xcprivacy)

```xml
NSPrivacyTracking: false
NSPrivacyTrackingDomains: (none)
NSPrivacyCollectedDataTypes: (none)
NSPrivacyAccessedAPITypes:
  - NSPrivacyAccessedAPIType: NSPrivacyAccessedAPICategoryUserDefaults
    NSPrivacyAccessedAPITypeReasons: ["CA92.1"]  // App functionality
```

---

## 6. Dependencies

| Package | Purpose | Size |
|---------|---------|------|
| `swift-markdown-ui` | Render markdown in read view | ~200KB |
| `KeychainAccess` | Store IAP receipt securely | ~50KB |

That's it. Two dependencies. No analytics SDK. No Firebase. No Crashlytics. No network libraries.

---

## 7. Monetization

### StoreKit 2

```swift
// One product
let proProductId = "com.yibi.etch.pro"

// Purchase flow
try await AppStore.sync()
let products = try await Product.products(for: [proProductId])
let result = try await products[0].purchase()
// Verify transaction, set isPro = true
```

### Free Tier Limits

- Max 50 entries (counted in main workspace, NOT burn folder)
- All features work in free tier (no feature paywall)
- When limit reached: gentle screen explaining Pro. No mid-entry popups. No dark patterns.
- Burn folder entries don't count against the 50-entry limit

### No Dark Patterns

- No "7-day trial" that charges automatically
- No "upgrade now!" popups while writing
- No fake urgency ("50% off today only!")
- No manipulative copy
- The paywall is on entry count. That's it.

---

## 8. Testing Strategy

| Area | Approach |
|------|----------|
| File operations | Unit tests: create, read, write, delete .md files in temp directory |
| Frontmatter parsing | Unit tests: parse/serialize entry metadata |
| Vibe mapping | Unit tests: vibe → color mapping, slider positions |
| Burn TTL | Unit tests: TTL expiry, auto-delete, manual burn |
| Redact | Unit tests: text replacement, permanence verification |
| Echo matching | Unit tests: vibe matching, keyword overlap, once-per-day logic |
| Color Cloud | Unit tests: monthly aggregation, color mapping |
| Aura video | Manual test: video generation, export, share |
| iCloud sync | Manual test: two devices, real iCloud account |
| StoreKit | StoreKit testing in Xcode (sandbox) |

---

## 9. Build Order for AI Agents

### Agent 1: File System Layer + Models
- `FileService`, `BookmarkService`, `Entry` model, `Workspace` model
- Folder picker, .md read/write, frontmatter parsing
- Test: create/write/read/delete entries in temp dir

### Agent 2: Core UI Shell
- `ThresholdView`, `CanvasView`, `VibeSliderView`, `EntryListView`
- Navigation, app state, settings shell
- Test: navigation flow, vibe slider color changes

### Agent 3: Wax Seal + Local Pulse + Onboarding
- `WaxSealView`, `LocalPulseView`, `EmptyStateView`, `FolderPickerView`
- Onboarding flow, save flow
- Test: seal animation, pulse animation, first launch

### Agent 4: Phase 2 Features
- `InternalWeatherView`, `EchoView`, `AntiJournalPromptView`, `BurnFolderView`, `RedactToolView`
- `EchoService`, `BurnService`, `PromptService`
- Test: weather painting, echo matching, prompt loading, burn TTL, redact

### Agent 5: Phase 3 Features + IAP
- `ColorCloudView`, `EndOfMonthAuraView`, `AuraRenderer`
- `ColorCloudService`, `IAPService`
- Test: color cloud aggregation, video export, StoreKit purchase

### Agent 6: Polish + App Store
- App icon, launch screen, App Store screenshots
- ASO keywords, description
- Final QA pass