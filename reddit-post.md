# Reddit-Post für r/macapps

> Flair: `[OS]` bzw. `App Showcase` (je nach aktueller Subreddit-Regel).
> Screenshots anhängen (Reihenfolge): 1. `site/assets/picker.png` · 2. `site/assets/hero-main.png` · 3. `site/assets/picker-help.png`
> Vor dem Posten: Download-/GitHub-Link einsetzen (unten markiert mit ⬅️).

---

## Titel

**[OS] Cortex — a keyboard-first clipboard manager for macOS that understands what you copy (on-device Apple Intelligence, plain Markdown storage, MCP server for Claude)**

---

## Post

I wanted to share **Cortex**, a native clipboard manager for macOS I've been building (Swift/SwiftUI, macOS 26).

The idea: your clipboard is probably the highest-signal data stream on your Mac — invoices, deal emails, meeting notes, links, tables — and most clipboard managers treat it as a dumb list. Cortex captures everything you copy and then actually *understands* it, entirely on-device.

### What it does

- **Capture everything** — text, links, tables, images, files. Every entry keeps its source app and timestamp. Duplicates get bumped to the top instead of duplicated.
- **On-device AI with "intents"** — you define situations like *Invoice* or *Deal Update*, each with its own prompt. Copy an invoice, hit ⌘K, and Cortex detects the intent and extracts vendor, amount, due date. A rambling deal email becomes stage + numbers + next step. Powered by Apple's foundation model — zero cloud calls.
- **Long content just works** — anything beyond the on-device context window is split into parts, processed per part and merged back into one result (map-reduce). No "content too long" dead ends.
- **Keyboard-first picker (⌘⇧V)** — fuzzy full-text search across content, titles, groups *and* AI summaries. Arrow keys + Enter to paste, ⌘1–9 for instant paste, ←/→ flips between raw content and the AI interpretation, ⌘? shows every shortcut right inside the picker.
- **Groups, not chaos** — ⌘⇧G files your latest copy into nested groups like `Sales/Q1`. Add ⌃ and the AI pipeline runs on it at the same time.
- **OCR + documents** — screenshots become searchable text (Vision, on-device). PDFs resolve via their text layer, scanned PDFs get per-page OCR, Word/RTF/ODT convert to clean text.
- **Plain Markdown storage** — every entry is a human-readable `.md` file (plus attachments) in a folder you choose. Point it at iCloud Drive and it syncs. `grep` it, version it, take it with you. No proprietary database, no lock-in.
- **MCP server included** — Claude Desktop (or any MCP client) can search, read, create and organize your clipboard library. One config snippet, runs locally over stdio.

### Privacy & Security

- Everything runs locally: capture, search, OCR, AI. No account, no telemetry, no server.
- Content from password managers is **never** captured (concealed/transient pasteboard types are respected).
- Full support for macOS 26 pasteboard privacy — Cortex plays by the new rules and guides you through the "Always Allow" setting instead of spamming alerts.
- Pause capture for 15 min / 1 h / indefinitely from the menu bar.

### Comparison

**Maccy**
- Fantastic, lightweight, open source.
- No AI understanding, no nested groups, no document/OCR resolution, history isn't portable plain-text.

**Paste**
- Beautiful UI, mature product.
- Subscription, closed storage format, no on-device AI interpretation, no MCP/automation story.

**Cortex**
- Understands what you copy (on-device intents, summaries, extraction).
- Your data = plain Markdown files you own.
- Keyboard-first everything, built-in shortcut overlay.
- MCP server → your clipboard becomes context for Claude & friends.

### Transparency

- **Requirements:** macOS 26+ (Apple Silicon). AI features need Apple Intelligence enabled — everything else works without it.
- **Cost:** Free right now (early-bird, 100% off). Core features are and will remain a **one-time lifetime purchase — no subscription, ever**. A subscription may come much later for optional premium add-ons only.
- **Download:** ⬅️ *LINK EINSETZEN*
- **Website:** ⬅️ *LINK EINSETZEN*

I'd love feedback — especially on the intent/prompt workflow and what you'd want the MCP server to expose. Happy to answer anything here.

---

## Erster Kommentar (direkt nach dem Posten selbst kommentieren)

Dev here — a few implementation notes for the curious:

- AI is Apple's FoundationModels framework (the ~3B on-device model). Intent detection is a constrained-decoding classify call with a strict yes/no confirmation pass so it doesn't force-match generic content.
- Long inputs get chunked at paragraph boundaries (≤ ~8k chars per part, up to 10 parts), each part runs the intent prompt, then one reduce pass merges the partial results.
- Storage is deliberately boring: front-matter + Markdown + a machine-readable state block per entry. Your library survives me abandoning this project (I won't, but that's the bar a clipboard tool should meet).
- The MCP server is a tiny self-contained stdio binary inside the app bundle — 8 tools (list/search/get/create/group/delete).

Ask me anything.
