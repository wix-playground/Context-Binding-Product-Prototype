# Repeater Architecture — Stakeholder Discussion

## 1. Where does the context live?

The data source that feeds a repeater — where is it attached?

| Option | Description | Pros | Cons |
|--------|-------------|------|------|
| **On the repeater** | Context is a property of the repeater itself | Simple mental model — "this repeater shows this data" | Not compatible with UoU interactions; the repeater itself can always use parent context anyway |
| **On the parent container** | Context lives on the section/page; repeater consumes it | Consistent with the way the system works. Decoupled — context is useful even without a repeater; other elements in the section can bind to it too (UoU interactions) | User must understand that data source ≠ repeater |

**Key question:** Is the data source a property of the repeater, or a property of the section that the repeater consumes?

---

## 2. Add Repeater flow

### 2a. What presets does "Add Repeater" offer?

| Option | Description | Behavior |
|--------|-------------|----------|
| **Presets without data** | Pre-designed templates (blog list, Articles grid, team cards) with no data connection — like a blank one | User gets a visual starting point, connects data later. On drop — all items show the same data |
| **Presets with unconfigured state** | Pre-designed templates that require data source selection before use | Blocked by a setup step — user must select a data source: existing / new / without (let me design) |

### 2b. What happens when a preset needs data?

| User choice | System behavior |
|-------------|-----------------|
| **Use existing content** | If a matching context exists on the parent → bind to it. If not → add the context to the parent, then bind |
| **Create new collection** | Create a new CMS collection, attach the context to the closest parent container, bind the repeater |
| **Use as blank** | No data connection — user gets a designable repeater with static placeholder content |

---

## 3. Where do sort & filter live?

| Option | Description | Implication |
|--------|-------------|-------------|
| **Context only** | Sort/filter are configured on the context instance | Two repeaters in the same context always show identical content. Need to solve the UX for 2 repeaters on the same section sharing the same context |
| **Context as base + per-repeater overrides** | Context provides base data; each repeater can override sort/filter at runtime | Better UX — one data source, multiple views. Filter & sort happen at runtime. Introduces a new type of component behavior (consumer-side transforms) |

**Follow-ups:**
- If per-repeater overrides — does the repeater settings panel show a mini config section, or a separate "display settings" area?
- If context-only — how do we handle the UX when two repeaters in the same section need different views of the same data?

---

## 4. Page size

Same pattern as sort & filter, with an added dimension:

| Option | Implication |
|--------|-------------|
| **On the context** | All repeaters sharing the context get the same page size and share pagination state |
| **Per-repeater** | Each repeater controls its own page size and has independent pagination controls |

---

## 5. Array binding — can a repeater bind to any array?

Can a repeater connect to any `arrayItems` field — including non-record, item-level arrays (e.g., tags, variants, reviews)?

Example: an `object` context (Current Product) exposes `reviews` as an array → can a repeater bind to it?

**If yes — two sub-questions:**

### 5a. Sub-context scoping

When bound to a nested array (e.g., `product.reviews → {author, rating, body}`), what do inner elements see?

| Option | Description |
|--------|-------------|
| **Only sub-context fields** | Inner elements can only bind to `author`, `rating`, `body` |
| **Sub-context + ancestor contexts** | Inner elements see item fields first, but can also access parent/page contexts (e.g., product name, logged-in user) |

### 5b. Sort, filter, page size for sub-arrays

If a repeater binds to a nested array, where does the user configure sort/filter/page size for it?

| Option | Description |
|--------|-------------|
| **Bind to a function** | The array field itself is a function that returns a filtered/sorted array — configuration happens in the context provider's code |
| **Repeater-level config** | The repeater exposes sort/filter/page size settings for the bound array (ties back to Q3) |

---

## 6. Context hierarchy — can the same context exist on parent and child?

Can a section have an instance of `cms.articles` when the page already has one?

| Option | Description | Impact |
|--------|-------------|--------|
| **No (strict prevention)** | Same context type cannot exist at both parent and child scope | Containers become the only tool for scoping multiple views of the same data |
| **Yes (shadowing)** | Child instance overrides parent for its subtree | More flexible, but users may not understand which instance they're binding to |

**Connection to question 3:** If sort/filter are per-repeater (not per-context), shadowing becomes less necessary — the same context instance can serve different views. If sort/filter are context-only, shadowing or containers are required for different views.

---

## Decision dependencies

```
Q1 (where context lives)
 └→ Q3 (sort/filter location)
     └→ Q4 (page size — same pattern)
         └→ Q6 (hierarchy/shadowing)
              └→ Q5 (array binding scope)

Q2 (add repeater flow) — independent, can be decided separately
```
