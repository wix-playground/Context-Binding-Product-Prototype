# Context Binding Platform — Use Cases List

**Total: 169 use cases | 57 prototyped | 17 partially done | 95 not started**

Last updated: March 10, 2026

---

## A — Adding & Managing Data Sources

Adding contexts, replacing, naming, scope, Add Panel, shadowing, multiple instances

| # | Use Case | Status |
|---|----------|--------|
| A01 | I want to add a data source (context) to my section | Done |
| A02 | I want to add a data source (context) to my page (different entry point) | Not started |
| A03 | I want to use my own content instead of the placeholder | Partial |
| A04 | I want to switch a list's data to a different context | Partial |
| A05 | I want to add a second data source to the same section | Partial |
| A06 | I want to add a pre-connected section from Add Panel (context exists) | Not started |
| A07 | I want to add a pre-connected section from Add Panel (new context) | Not started |
| A08 | I want to use a Wix App as my data source (Stores, Bookings) | Not started |
| A09 | I want to add a custom code context (Weather, Countdown) | Not started |
| A10 | I want to detach a context from my page or section | Partial |
| A11 | I want to rename a context instance to something meaningful | Done |
| A12 | I want to know what data a context exposes | Done |
| A13 | I want to see which elements are using a specific context | Done |
| A14 | I want to share a context across all sections on the page | Partial |
| A15 | I want two different views of the same collection | Done |
| A16 | Add context to section when same collection already on page (shadowing) | Partial |
| A17 | I want to use data in the header/footer (global section) | Not started |
| A18 | I want to extend an app's data with my own custom fields | Not started |

---

## B — Binding Properties to Data

Connect, disconnect, cross-context, object binding, functions, formatters, panel UX, visibility, a11y, rich text, rich content, link config, empty fields

### Core Binding Actions

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B01 | Connect a property to a data field | Done | CNF |
| B02 | Disconnect a property and return to static | Done | CNF |
| B03 | See which context and field a property is connected to | Done | CNF |
| B04 | Navigate from bound property to context settings | Done | CNF |
| B05 | Understand on canvas which elements are connected vs static | Done | CNF |
| B06 | Bind from ancestor context (cross-context) | Partial | CNF |
| B07 | Bind to an entire object (not a single field) | Not started | High |

### Editing Bound Content

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B08 | Edit content bound to a collection (table data) | Done | CNF |
| B09 | Understand computed content (function result) | Partial | High |

### Binding Without Context

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B10 | Bind when no context available — add from binding flow | Done | CNF |

### Functions & Formatters

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B11 | Connect a property to a function | Done | CNF |
| B12 | Pass parameters to a function (static or bound) | Done | High |
| B13 | Format date, time, or date+time field | Not started | High |
| B14 | Format a link / URL field | Not started | High |
| B15 | Connect navigation to a dynamic page | Not started | High |

### Binding Panel UX

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B16 | Choose context from source dropdown | Done | CNF |
| B17 | Choose field from values dropdown | Done | CNF |
| B18 | Understand field type compatibility | Done | CNF |
| B19 | No compatible fields — what happens | Not started | High |
| B20 | See sample / preview data when selecting | Done | NTH |

### Specific Property Types

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B21 | Show/hide element based on data | Partial | High |
| B22 | Bind alt text for image accessibility | Done | High |
| B23 | Mark image as decorative (no alt) | Partial | NTH |
| B24 | Bind accessibility properties to data (aria-label, role) | Not started | High |
| B25 | Bind rich text field to element | Not started | High |
| B26 | Bind rich content field to element | Not started | High |

### Link Binding & Configuration

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B27 | Configure link target (current tab / new tab) for bound URL | Not started | High |
| B28 | Configure link type for bound URL (page / web / anchor / email / phone) | Not started | High |
| B29 | Configure SEO rel attributes for bound links (nofollow, noopener) | Not started | High |

### Empty & Missing Data

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| B30 | Manage behavior for empty bound fields | Not started | High |

---

## C — Repeater & List Renderers

Repeater core, auto-bind, canvas states, design, operations, Gallery, Table, Accordion, Maps

### Repeater Core

| # | Use Case | Status |
|---|----------|--------|
| C01 | Add a repeater and connect it to my collection | Done |
| C02 | Connect inner elements to fields | Done |
| C03 | Connect inner elements when "Items" is not bound | Not started |
| C04 | Switch repeater to a different data source | Partial |
| C05 | Disconnect repeater from data | Partial |
| C06 | "Items" auto-binds when context attached | Done |
| C07 | Canvas: "Items unbound" vs "no context at all" | Not started |
| C08 | "Items" connected to a different context than repeater's | Not started |

### Repeater Design & Layout

| # | Use Case | Status |
|---|----------|--------|
| C09 | Add blank repeater, design before connecting | Not started |
| C10 | Add pre-designed repeater from presets | Not started |
| C11 | Two repeaters, same data, different filters | Done |
| C12 | Bind repeater to nested array (tags, reviews) | Not started |
| C13 | A/B alternating layout pattern | Not started |
| C14 | Repeater inside repeater (nested lists) | Not started |

### Repeater Operations

| # | Use Case | Status |
|---|----------|--------|
| C15 | Copy-paste repeater with bindings | Not started |
| C16 | Drag repeater to different section | Not started |

### More List Components

| # | Use Case | Status |
|---|----------|--------|
| C17 | Gallery as data-driven list | Not started |
| C18 | Table component with list data | Not started |
| C19 | Accordion / Tabs with list data | Not started |
| C20 | Slideshow driven by list data | Not started |
| C21 | Google Maps driven by list of locations | Not started |

---

## D — Context Configuration

Filter, sort, random order, page size, app settings, bindable config, reset

| # | Use Case | Status |
|---|----------|--------|
| D01 | Filter the list to show specific items | Done |
| D02 | Sort my list by a specific field | Done |
| D03 | Show items in random order | Not started |
| D04 | Limit how many items load per page | Not started |
| D05 | Add multiple filter rules (AND logic) | Partial |
| D06 | Edit or delete an existing filter rule | Partial |
| D07 | Sort by multiple fields | Not started |
| D08 | Configure app-specific settings (currency, units) | Not started |
| D09 | Bind config property to data (dynamic filter) | Not started |
| D10 | Filter by current page context value | Done |
| D11 | Reset config to default | Not started |

---

## E — Reference Fields

Single ref, multi-ref, reverse ref, self-ref, dynamic filtering, not-in

| # | Use Case | Status |
|---|----------|--------|
| E01 | Show referenced field in repeater | Done |
| E02 | Show details from referenced item on item page | Done |
| E03 | Show list of multi-referenced items | Done |
| E04 | Show reverse-referenced items | Done |
| E05 | Filter by reference field | Done |
| E06 | Exclude current and related items | Done |
| E07 | Filter list by current page item | Done |
| E08 | Self-referencing items | Done |

---

## F — Functions, Actions & Event Handlers

Function binding, params, events, action chaining, OOTB libraries, conflict resolution

| # | Use Case | Status |
|---|----------|--------|
| F01 | Bind text to a formatting function | Done |
| F02 | Pass static parameters to function | Done |
| F03 | Pass context field as function parameter | Done |
| F04 | Bind button click to action (addToCart) | Not started |
| F05 | Bind page event to function | Not started |
| F06 | See available function libraries | Done |
| F07 | Function combining values from multiple contexts | Not started |
| F08 | Bind repeater event to navigation | Not started |
| F09 | Chain multiple actions on one event | Not started |
| F10 | Use OOTB Wix function libraries | Not started |
| F11 | Custom action vs built-in behavior conflict | Not started |

---

## G — UoU Interactions (Visitor Controls)

Search, sort, pagination, load more, infinite scroll, empty states, loading, conditional filtering

| # | Use Case | Status |
|---|----------|--------|
| G01 | Search input that filters repeater | Not started |
| G02 | Sort buttons for visitors | Not started |
| G03 | Pagination (next/prev) | Not started |
| G04 | Show total number of items | Not started |
| G05 | "Load more" button | Not started |
| G06 | Category filter tabs | Not started |
| G07 | Loading state indicator | Not started |
| G08 | "No results" empty state | Not started |
| G09 | Promote context for external controls | Not started |
| G10 | Infinite scroll | Not started |
| G11 | Conditional / dependent filter options | Not started |
| G12 | Input component as filter | Not started |
| G13 | Input component as sort control | Not started |

---

## H — Multi-State Box (MSB)

Static design, connected mode, remap, join, default view, canvas dropdown

| # | Use Case | Status |
|---|----------|--------|
| H01 | Design multiple views/states | Done |
| H02 | Connect MSB to states field | Done |
| H03 | Remap which state shows which design | Done |
| H04 | Join two data states to same design | Done |
| H05 | Set fallback/default view | Done |
| H06 | Disconnect and go back to static | Done |
| H07 | Add/delete/duplicate/reorder views | Done |
| H08 | Switch to a different states field | Partial |
| H09 | See active view per state on canvas | Done |

---

## I — Dynamic Pages (Item Pages)

Primary context, related items, page generation, URL binding, page variants, app pages

| # | Use Case | Status |
|---|----------|--------|
| I01 | Create item page per entity | Done |
| I02 | Bind elements to current item's fields | Done |
| I03 | Repeater of related items on item page | Done |
| I04 | Filter which items get their own page | Not started |
| I05 | Show "Other items" excluding current | Done |
| I06 | Navigate from list to item page | Not started |
| I07 | Add suggested repeaters from Add Elements panel | Done |
| I08 | URL binding / SEO slug from data | Not started |
| I09 | Different layouts for same page based on data | Not started |
| I10 | App-owned dynamic page with app context | Not started |

---

## J — Scope, Hierarchy & Lifecycle

Scope resolution, shadowing, promote, remove, move, duplicate, system contexts, lightbox, missing context

| # | Use Case | Status |
|---|----------|--------|
| J01 | Understand available data sources for an element | Done |
| J02 | Override page-level context with section-specific one | Partial |
| J03 | Promote context from repeater to section | Not started |
| J04 | Remove context and understand impact | Not started |
| J05 | Move section to another page | Not started |
| J06 | Duplicate section with data connections | Not started |
| J07 | Use system context (Identity, Business Info) | Not started |
| J08 | Bind to logged-in user's name | Not started |
| J09 | Use data inside a Lightbox | Not started |
| J10 | Guidance when component needs missing context | Not started |

---

## K — Add Elements Panel & Presets

Suggested elements, drag pre-bound, preset gallery, new collection, contextual panel

| # | Use Case | Status |
|---|----------|--------|
| K01 | Suggested elements based on page data | Done |
| K02 | Drag suggested element pre-connected | Partial |
| K03 | Pre-designed repeater from preset gallery | Not started |
| K04 | Create new collection from preset schema | Not started |
| K05 | Contextual add panel per active context | Not started |

---

## L — Canvas & Visual Feedback

Canvas pills, chip navigation, scope badges, MSB indicators, live preview

| # | Use Case | Status |
|---|----------|--------|
| L01 | Pills on bound elements | Done |
| L02 | Click chip to navigate to context settings | Done |
| L03 | MSB states switching on canvas | Done |
| L04 | See bound vs static repeater items | Partial |
| L05 | Scope badges | Done |
| L06 | MSB border style per connection state | Done |
| L07 | Live Preview with real data | Not started |

---

## M — Design Variants & Conditional Styling

Data-driven styling, CSS switching, conditional badges, layout variants

| # | Use Case | Status |
|---|----------|--------|
| M01 | Change element design based on data ("out of stock") | Not started |
| M02 | Apply different CSS/animation based on field value | Not started |
| M03 | Show "Sale" badge only when discount exists | Not started |
| M04 | Switch layout variants based on item state | Not started |

---

## N — Write Mode & Forms

Form submission, data entry, reviews, profile updates, validation, permissions

| # | Use Case | Status |
|---|----------|--------|
| N01 | Create form that submits to collection | Not started |
| N02 | Bind input to collection field for data entry | Not started |
| N03 | Visitors submit reviews | Not started |
| N04 | Visitors update their profile | Not started |
| N05 | Validation from data schema | Not started |
| N06 | Custom form from individual inputs | Not started |
| N07 | Read vs write permissions | Not started |

---

## O — Custom Components & Developer Extensibility

Custom components, Velo/React contexts, inline field creation, AI-first setup

| # | Use Case | Status |
|---|----------|--------|
| O01 | Custom component that binds to ancestor context | Not started |
| O02 | Custom component with its own context | Not started |
| O03 | Build custom context via Velo/React | Not started |
| O04 | Add CMS field from binding flow | Not started |
| O05 | AI handles wiring and setup | Not started |

---

## P — CMS-Specific Flows

Schema editing, usage overview, collection management, broken bindings from CMS changes

| # | Use Case | Status | Priority |
|---|----------|--------|----------|
| P01 | Add new field from context panel | Not started | |
| P02 | See which collections connect where | Not started | |
| P03 | CMS field deleted that was bound to elements (broken binding) | Not started | High |
| P04 | CMS collection deleted that was connected to pages (broken context) | Not started | High |

---

## Out of Scope — This Phase

**DB Drivers** — Backend data connectivity layer. Mentioned in the high-level architecture but not part of the current context binding product scope. Will be addressed in a future phase.

## TBD — Requires R&D Sign-Off

**Context filter = hard ceiling for UoU interactions** — Context-level filters set by the site builder define the maximum dataset boundary. When UoU filters are applied at runtime by the site visitor (search, sort, filter dropdown), they can only narrow the results — never widen them. The visitor cannot discover or access items that were filtered out at the context level.

**R&D implication:** The runtime query pipeline must enforce context-level filters as a hard constraint. UoU filters are intersected with (applied on top of) the context filter, never replace it. This must be guaranteed regardless of implementation (client-side, server-side, or hybrid). *Requires explicit R&D approval.*
