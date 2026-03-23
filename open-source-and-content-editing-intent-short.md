# Open Source & Content Editing — Short (Meeting Deck)

## 1. Open Source (context level)

**User intent:** "I want to see the full list" / "I want to see the code."

| Source | Opens to | CTA label |
|--------|----------|-----------|
| CMS | CMS Collection | "Open Collection" |
| Wix Apps (Verticals) | App dashboard (e.g. list) | "Open in Dashboard" |
| Custom Code | Context code file | "Open Source File" |
| **TPAs** | TBD | TBD |

**Entry points** (shared across all source types):
- Context card — direct link
- Context card — 3-dots menu
- Context Configuration panel
- Context Explorer modal

> **Note:** Function Libraries are not contexts — they appear only at the property/binding level (see Section 2).

---

## 2. Content & logic editing (element / cell level)

**User intent:** "I see a typo / wrong image" → fix **content**. "Wrong value from my function" → fix **logic**.

**Entry point:** Settings panel, property controller — link next to the bound chip (Value, Source, etc.).

| Source | Binding Type | CTA on entry point | What opens |
|--------|--------------|--------------------|------------|
| CMS / App Context | Data (e.g. title, image) | **"Edit in Dashboard"** | CMS / App — specific cell |
| | Calculated Data (e.g. readingTime from content) | N/A | N/A — value is computed by the context provider, not directly editable |
| | Action (e.g. addToCart) | N/A | N/A |
| | State (e.g. visibility state) | N/A | N/A |
| User-generated Custom Context | Data & Functions | **"&lt;/&gt; Open source file"** | Code — specific function |
| **Function Library** | Custom Function Library | **"&lt;/&gt; Open source file"** | Function code file — specific function |
| | System Function Library | N/A | No way to open or see — bindable but not editable |
| | Installed App Function Library | N/A | No way to open or see — bindable but not editable |

---

## 3. Content editing (item level)

**User intent:** "I want to add an item" / "I want to see all the data of this item."

| Source | Entry point | What opens |
|--------|-------------|------------|
| Dynamic Page (CMS / App) | Page item switcher — "Edit Item" | Item page in Dashboard |
| | Page item switcher — "Add new Item" | New item page in Dashboard |
| List Context | Manage Items Panel (TBD) — Edit Item | Item page in Dashboard |
| | Manage Items Panel (TBD) — Add new item | New item page in Dashboard |

---

## Open Questions

1. **Manifest for "open" target** — Can the context provider declare the "open source" target in its manifest? (e.g. URL or deep link for "Open Collection", "Open in Dashboard", or "Open code file"). Need to create accurate guidelines.

2. **Distinguishing editable data** — Can we distinguish what binding is actually data we can direct the user to edit in the dashboard — vs an action / calculated data / state?

3. **TPAs** — How "Open Source" and "Edit" behave when the context is provided by a TPA. Manifest for "open" target? Item/repeater edit entry points?

4. **No-source use cases** — Research & define use cases without a source to open — e.g. system function libraries, installed app function libraries.

5. **Custom Code context backed by CMS** — A developer writes a Custom Code context that wraps a CMS collection (e.g. "Featured Articles" that reads from Articles but applies custom ranking). Some fields are passthrough CMS data (editable in dashboard), others are computed by code (not editable). Today the platform sees only "Custom Code" as the source — it can't distinguish CMS-backed fields from computed ones. Should the manifest support declaring `dataSource`, `collectionId`, and per-field editability? This affects: "Open Source" behavior (code vs. collection?), "Edit in Dashboard" affordance (per field?), permission visibility, and auto-filter/relationship detection.
