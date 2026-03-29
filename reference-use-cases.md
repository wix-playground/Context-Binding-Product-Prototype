# Reference & Multi-Reference — Use Cases

> Comprehensive list of user scenarios involving relationships between collections.
> Covers single reference, multi-reference, reverse reference, self-reference, and transitive patterns.
> Each use case represents a distinct user intent that the platform needs to support.

---

## How to read this

- **Type** — the kind of relationship involved
- **User intent** — what the user is trying to do
- **Example** — a concrete scenario using real-world collections
- **Status** — `✅ Prototyped` | `🔶 Partially` | `⬜ Not started`

---

## R1 — Displaying Referenced Data (Read)

The user has related collections and wants to show data from one collection inside the context of another.

### Single Reference (one-to-many)

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R1.01 | I want to show a field from a referenced item inline in a list | Team Members list → show each member's **role name** (from Roles collection) | ✅ |
| R1.02 | I want to show multiple fields from a referenced item on a detail page | Team Member page → show role **name**, **description**, and **icon** | ✅ |
| R1.03 | I want to show a referenced item's **image** (not just text fields) | Team Member page → show the role's **badge image** from Roles collection | ⬜ |
| R1.04 | I want to show a field from a reference **two levels deep** | Article → Category → Department name (Article has a `category` ref, Category has a `department` ref) | ⬜ |

### Multi-Reference (many-to-many)

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R1.05 | I want to show a list of multi-referenced items for each item in a repeater | Books list → for each book, show its **authors** as a nested repeater | ✅ |
| R1.06 | I want to show multi-referenced items on an item page | Book page → show all **authors** of this book with their details | ✅ |
| R1.07 | I want to show a **count** of multi-referenced items without showing the full list | Books list → show "3 authors" as text, without rendering each author | ⬜ |
| R1.08 | I want to show a **comma-separated list** of referenced item names (not a repeater) | Book card → "By Alice, Bob, Charlie" as a single text element | ⬜ |

### Reverse Reference

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R1.09 | I want to show all items that reference the current item | Role page → show all **team members** with this role | ✅ |
| R1.10 | I want to show all items that multi-reference the current item | Author page → show all **books** by this author | ✅ |
| R1.11 | I want to show a count of reverse-referenced items | Roles list → show "12 members" next to each role | ⬜ |

### Self-Reference (same collection)

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R1.12 | I want to show related items from the same collection | Book page → show **related books** (self-referencing multi-ref) | ✅ |
| R1.13 | I want to show a parent item from the same collection | Employee page → show this employee's **manager** (self-ref single) | ⬜ |
| R1.14 | I want to show child items from the same collection (tree) | Category page → show **sub-categories** (reverse self-ref) | ⬜ |

### Transitive (chain of references)

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R1.15 | I want to show items reachable through a chain of references | Book page → "More by same author(s)" (Book → Authors → Authors' other Books) | ✅ |
| R1.16 | I want to show items that share a common reference | Author page → "Co-authors" — other authors who share a book with this author | ⬜ |

---

## R2 — Controlling Referenced Lists (Sort, Filter, Paginate)

The user has a list of referenced items (from multi-ref, reverse-ref, or transitive) and wants to control how it's displayed.

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R2.01 | I want to **sort** a multi-reference list (e.g. alphabetically) | Book page → authors sorted **A → Z** | ⬜ |
| R2.02 | I want to **filter** a multi-reference list | Recipe page → show only **vegetarian** ingredients from the ingredients multi-ref | ⬜ |
| R2.03 | I want to **limit** how many referenced items are shown | Book card → show only the **first 3 authors**, with "+2 more" | ⬜ |
| R2.04 | I want to **paginate** a long referenced list | Author page → show books with **Load more** (author has 50+ books) | ⬜ |
| R2.05 | I want to sort a reverse-reference list | Role page → team members sorted by **join date** | ⬜ |
| R2.06 | I want to filter a reverse-reference list | Role page → show only **active** team members with this role | ⬜ |
| R2.07 | I want the visitor to sort/filter a referenced list at runtime (UoU) | Author page → visitor sorts books by **date** or **rating** | ⬜ |

---

## R3 — Filtering BY Reference Fields

The user wants to use a reference relationship as a filter condition on a list.

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R3.01 | I want to filter a list by a **static** reference value | Team Members list → show only members where `role = "UX Designer"` | ✅ |
| R3.02 | I want to filter a list by the **current page's item** (dynamic) | Role page → Team Members filtered by `role = Current Role` | ✅ |
| R3.03 | I want to **exclude** the current item from a list | Book page → "Other books" excluding the current book | ✅ |
| R3.04 | I want to **exclude** items that appear in a multi-ref array | Book page → "All other books" excluding current **and** related books (`not_in`) | ✅ |
| R3.05 | I want to **include** items that share a multi-ref value | Book page → "More by same author" — books that share at least one author (`shares_any`) | ✅ |
| R3.06 | I want to filter by a reference field in a **visitor-facing control** (UoU) | Articles list → dropdown to filter by **category** (reference to Categories collection) | ⬜ |
| R3.07 | I want to filter by a **sub-field** of a reference (not the reference itself) | Team Members filtered by `role.department = "Engineering"` | ⬜ |
| R3.08 | I want to combine reference filters with regular field filters | Articles where `category = "Tech"` AND `publishDate > 2024` | 🔶 |

---

## R4 — Nested Repeaters with Referenced Data

The user has a repeater inside a repeater, where the inner repeater displays referenced data.

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R4.01 | I want a nested repeater showing multi-ref items per parent item | Articles repeater → for each article, an inner Authors repeater | ✅ |
| R4.02 | I want inner elements to bind to both **item-level** and **parent-level** fields | Inner author card shows `authors.name` (item) and `items.title` (parent article) | ✅ |
| R4.03 | I want the inner repeater to have its own **sort** independent of the parent | Outer: articles by date. Inner: authors A → Z | ⬜ |
| R4.04 | I want the inner repeater to have its own **filter** independent of the parent | Outer: all articles. Inner: only authors with `isMainAuthor = true` | ⬜ |
| R4.05 | I want an inner repeater with a **separate context** (not a sub-array) for full control | Inner Authors repeater with its own Authors context, auto-filtered by current article | 🔶 |
| R4.06 | I want to **upgrade** from a simple sub-array binding to a full context when I need more control | Started with `items.authors` array, now want sort/filter → system suggests adding a separate Authors context | ⬜ |
| R4.07 | I want to disconnect the outer repeater and understand what happens to inner bindings | Disconnect Articles → inner Authors repeater behavior depends on binding type (sub-array vs own context) | ✅ |
| R4.08 | I want **3 levels** of nesting — repeater in repeater in repeater | Departments → Teams → Members (Department list → per department a Teams repeater → per team a Members repeater) | ⬜ |

---

## R5 — Relationship Setup & Discovery

How the user establishes, discovers, and manages relationships between contexts.

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R5.01 | I want the system to **auto-detect** that two contexts are related | Articles context and Authors context share a multi-ref field → system knows they're related | 🔶 |
| R5.02 | I want to see a **relationship indicator** on the context card | Authors context card shows "Filtered to match the current Articles item" with a toggle | ✅ |
| R5.03 | I want to **manually define** a relationship between two contexts when auto-detect fails | Context A has a field that matches Context B's ID, but system doesn't auto-detect → user sets it up | ⬜ |
| R5.04 | I want the **Add Elements panel** to suggest repeaters based on my page's reference fields | Book page → panel suggests "Authors", "Related Books", "All Other Books" as pre-bound elements | ✅ |
| R5.05 | I want to understand **which approach to use** — sub-array binding or separate context | System guides: "For simple display, bind to the array. For sort/filter/pagination, add a separate context" | ⬜ |
| R5.06 | I want to add a context that is **auto-filtered by the parent repeater's current item** | Add Authors context to inner repeater → auto-filter `WHERE article = current article` is applied | 🔶 |
| R5.07 | I want to **remove** a relationship (turn off auto-filter) without removing the context | Authors context stays, but stops filtering by current article → shows all authors | 🔶 |
| R5.08 | I want to see **all relationships** on a page at a glance | A page-level view showing: "Articles → Authors (multi-ref), Articles → Categories (single ref)" | ⬜ |

---

## R6 — Edge Cases & Advanced Patterns

| # | User Intent | Example | Status |
|---|------------|---------|--------|
| R6.01 | I want to handle **empty references** gracefully | Team member with no role assigned → role fields show empty/fallback, not an error | ⬜ |
| R6.02 | I want to handle a **broken reference** (referenced item was deleted) | Author was deleted from Authors collection → book's author list shows N-1 authors, no error | ⬜ |
| R6.03 | I want to show referenced data from **different providers** (not just CMS) | Products (Wix Stores) → Related Articles (CMS) — cross-provider reference | ⬜ |
| R6.04 | I want a **circular reference** to not cause infinite loops | Employee → Manager → Manager's Manager → … (same collection, chain of self-refs) | ⬜ |
| R6.05 | I want to **create a new referenced item** inline (without leaving the page) | Editing a book → add a new author directly from the authors picker | ⬜ |
| R6.06 | I want to **reorder** multi-referenced items manually (drag in CMS) and have that order reflected | Book's authors ordered by "importance" in the CMS → repeater shows that order by default | ⬜ |
| R6.07 | I want to bind to a reference field from a **function library** (computed reference) | Function returns a "recommended product" based on user behavior → bind as if it were a reference | ⬜ |

---

## Summary

| Section | Total | ✅ | 🔶 | ⬜ |
|---------|-------|----|-----|-----|
| **R1** — Displaying Referenced Data | 16 | 7 | 0 | 9 |
| **R2** — Controlling Referenced Lists | 7 | 0 | 0 | 7 |
| **R3** — Filtering BY References | 8 | 5 | 1 | 2 |
| **R4** — Nested Repeaters | 8 | 3 | 1 | 4 |
| **R5** — Relationship Setup | 8 | 2 | 3 | 3 |
| **R6** — Edge Cases & Advanced | 7 | 0 | 0 | 7 |
| **TOTAL** | **54** | **17** | **5** | **32** |

---

## Key Decision Points These Use Cases Surface

1. **R2 (Sort/Filter on referenced lists)** — This is the core question from the Options 1/2/3 discussion. Which approach? Sub-array without controls, separate context, or array-level sort/filter?

2. **R4.06 (Upgrade path)** — Can the user start simple (Option 1 — bind to array) and upgrade to advanced (Option 2 — separate context) without losing their work?

3. **R5.01 (Auto-detect relationships)** — How smart is the system at recognizing that two contexts are related? Is it schema-based (matching reference fields) or does the user always have to set it up?

4. **R1.04 (Deep references)** — How many levels of auto-resolve? One level (current) or recursive?

5. **R6.03 (Cross-provider references)** — References between CMS and App contexts (e.g., Products → Articles) — same mechanism or different?
