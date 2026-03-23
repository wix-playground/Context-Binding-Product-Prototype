# Input Component Binding — Product Specification

> Defines how input components (Dropdown, TextInput, Checkbox, etc.) bind to contexts and function libraries. Covers the binding model, intent-driven behavior, panel UX, and options derivation for filter, sort, form, and product option use cases.
>
> Complements [context.md](./context.md) (core concepts, binding, scoping) and [contextConfig.md](./contextConfig.md) (design-time configuration).

---

## Core Challenge

The existing binding system is **unidirectional read**: component property ← context field. Input components introduce **bidirectional binding** — the component both reads state from and writes state to a context. The meaning of the write changes based on intent.

A single Dropdown component can serve multiple intents across the site:

| Intent | Read (display) | Write (on selection) | Example |
|--------|---------------|---------------------|---------|
| **Filter control** | Enum values from schema field | Apply/update filter on list context | Filter articles by category |
| **Sort control** | Sortable fields × directions (curated) | Apply/update sort on list context | Sort articles by date |
| **Form field — create** | Enum values from schema or static list | Set value on form state (deferred) | Select category for new article |
| **Form field — update** | Same + pre-fill with current value | Update field on existing record (deferred) | Change article category |
| **Product option selector** | Variant options from product context | Set variant selection on product context | Select product size before add-to-cart |

---

## Architecture Decisions

### One component per input type, not per intent

There is one Dropdown component, one TextInput component, one Checkbox component, etc. Different intents are not different components. The component manifest is the same regardless of whether the Dropdown is used for filtering, sorting, or form input.

**Rationale:** Fewer components in the add panel; consistent with how display components already work (one Text component, many uses); the user can change intent by rebinding.

### The function bound to `onChange` IS the role

The intent is determined by which function the user binds to the component's `onChange` event — not by an explicit role selector or a special configuration property.

- `onChange` → `applyFilter(...)` → the Dropdown becomes a filter control
- `onChange` → `applySort(...)` → the Dropdown becomes a sort control
- `onChange` → `setFormField(...)` → the Dropdown becomes a form field (deferred)
- `onChange` → `selectOption(...)` → the Dropdown becomes an option selector (deferred)

This reuses the existing **function binding pattern** (same mechanism as Button `onClick` → `addToCart()`). No new binding primitives needed.

### The panel adapts based on the binding target

The input component uses a **custom panel** (not the auto-panel) that adapts its sections based on what `onChange` is bound to. Different functions surface different parameter configurations and options behaviors.

### Context provides the functions; component is generic

The list context exposes filter/sort actions as functions in `context.items`. The product context exposes option selection actions. The component is a generic UI control that calls whatever function it's bound to. The context is the source of truth for behavior.

---

## The Dropdown's Two Properties

An input component (using Dropdown as the primary example) has two key bindable properties:

| Property | Direction | Mechanism | Purpose |
|----------|-----------|-----------|---------|
| `options` | Read | Property binding (same as Text.value) | Where the list of choices comes from |
| `onChange` | Write | Function binding (same as Button.onClick) | What happens when the visitor selects — determines the role |

Both use existing binding primitives. No new concepts required.

### The `event` parameter (auto-filled)

The `onChange` function receives the visitor's selection via a `CHANGE` domEvent parameter (already defined in the functions spec):

```json
{
  "domEvent": "CHANGE",
  "changeEvent": { "inputType": "text" }
}
```

This parameter is auto-filled by the component — the user does not bind it. It appears grayed out in the panel. The value comes from `event.target.value` at runtime.

---

## Options: Three Sources, Priority Rules

The Dropdown's `options` can come from three sources:

| Source | When | Example |
|--------|------|---------|
| **From function** (locked) | `onChange` is bound, and the function implies known values | Filter by category → enum values from schema |
| **Bound to context field** | User explicitly binds the `options` property | Bound to `tags` array from a different context |
| **Static / manual** | User defines options by hand in the panel | "Small", "Medium", "Large" typed manually |

### Priority

- When `onChange` is bound and the function implies known values → options are **locked** (derived from function, not editable). The user cannot override them.
- When `onChange` is bound but the function cannot derive options → user must bind or define options explicitly.
- When `onChange` is not bound → user binds options or defines a static list.

**Rationale for locking:** The function defines the contract. If `applyFilter` is set to field = "category", the valid values ARE the category enum values. Showing different options would break the contract.

---

## Intent: Filter

### Context exposes `applyFilter` as a function

The list context (e.g., `cms.articles`) exposes `applyFilter` in its `context.items` as an action:

```json
{
  "applyFilter": {
    "dataType": "function",
    "category": "actions",
    "function": {
      "parameters": [
        {
          "domEvent": "CHANGE",
          "changeEvent": { "inputType": "text" },
          "optional": true,
          "displayName": "Selected value"
        },
        {
          "dataType": "text",
          "displayName": "Field to filter"
        },
        {
          "dataType": "textEnum",
          "displayName": "Condition",
          "textEnum": {
            "options": [
              { "value": "equals", "displayName": "Equals" },
              { "value": "contains", "displayName": "Contains" },
              { "value": "startsWith", "displayName": "Starts with" }
            ]
          }
        }
      ]
    }
  }
}
```

### Binding flow

1. User adds a Dropdown to the page
2. User binds `onChange` → picks `applyFilter` from the Articles context
3. User sets the `field` parameter → picks "category"
4. System recognizes "category" is a `textEnum` with known values → **auto-populates and locks** the Dropdown's options to `["Tech", "Design", "Business"]`
5. Dropdown shows options on canvas immediately

### Options derivation

When the `field` parameter points to a schema field:
- **textEnum field** → options = the enum's predefined values (locked)
- **Plain text or other field with no predefined values** → options cannot be auto-derived; user must bind or define them manually

### "All" option (reset filter)

The `CHANGE` event parameter has `optional: true`. This signals to the component that "no value" is valid — meaning "clear this filter." The component renders an "All" option at the top of the list.

- When the visitor selects "All", the component passes a null/empty value to the function
- The function clears the filter for that field
- This only appears when the event parameter is optional — sort, form, and product options don't show "All" when their event parameter is required

### Baseline interaction

The editor user's design-time filter (set in the context config panel) is the **baseline**. The visitor's filter selection **layers on top**:

- Editor sets: `featured = true` (in config panel)
- Visitor selects: `category = "Tech"` (via Dropdown)
- Result at runtime: `featured = true AND category = "Tech"`

The visitor can never override or remove the editor's baseline filter.

### Panel layout (filter mode)

```
┌──────────────────────────────────────────────┐
│ Options                                       │
│ │ From "category": Tech, Design, Business  │  ← locked, informational
├──────────────────────────────────────────────┤
│ On Change                                     │
│ [📎 Articles] [applyFilter  ✕]               │
│                                               │
│ Parameters                                    │
│ │ Selected value        (auto from input)  │  ← grayed out
│ │ Field to filter       [category      ▾]  │  ← user picks
│ │ Condition             [Equals        ▾]  │  ← user picks
├──────────────────────────────────────────────┤
│ Label: [Category ▾]           ← bindable      │
│ Placeholder: [Select...]      ← static        │
│ Disabled: [ ]                 ← bindable       │
│ Visibility: [ ]               ← bindable       │
└──────────────────────────────────────────────┘
```

### Multiple inputs filtering the same context

Multiple input components can filter the same list context. Each controls its own slot:

- Dropdown A → `applyFilter(field: "category", ...)`
- Slider B → `applyFilter(field: "price", ...)`
- TextInput C → `applyFilter(field: "title", condition: "contains")`

The context holds the composed state internally (AND logic). Each input reads its own current value and writes to its own slot.

**Two apply modes:**

| Mode | Behavior | How it works |
|------|----------|-------------|
| **Immediate** (onChange) | Each input applies its filter on selection | Each input calls `applyFilter` directly; context re-filters on each change |
| **Deferred** (apply button) | Inputs write to pending state; an "Apply Filters" button commits all at once | Inputs call a pending version; a separate `commitFilters()` action applies them |

When multiple inputs filter the same context, they also **filter each other** — selecting a category may reduce the available options in a dependent dropdown (cascading filters).

---

## Intent: Sort

### How sort differs from filter

| | Filter | Sort |
|--|--------|------|
| Options represent | Values of ONE field | Fields × directions (compound) |
| Auto-derivation | From one field's enum values | From all sortable fields × 2 directions |
| User control | Locked — all values shown | **Curated** — check/uncheck + reorder |

### Context exposes `applySort` as a function

```json
{
  "applySort": {
    "dataType": "function",
    "category": "actions",
    "function": {
      "parameters": [
        {
          "domEvent": "CHANGE",
          "changeEvent": { "inputType": "text" },
          "optional": true,
          "displayName": "Selected sort"
        }
      ]
    }
  }
}
```

The event parameter carries the composite sort key (e.g., `"date_desc"`). `optional: true` allows a "Default" reset option.

### Options: curated from schema

When `onChange` is bound to `applySort`, the system auto-generates all sortable field × direction combinations from the schema. The user **curates** this list by checking/unchecking options and reordering them.

**Auto-generated with smart labels based on field type:**

| Field type | Ascending label | Descending label |
|-----------|----------------|-----------------|
| Text | A → Z | Z → A |
| Date | Oldest first | Newest first |
| Number | Low → High | High → Low |
| Boolean | No first | Yes first |

**Example for Articles collection:**

| Option | Value | Default state |
|--------|-------|--------------|
| Date: Newest first | `date_desc` | ☑ Enabled |
| Date: Oldest first | `date_asc` | ☑ Enabled |
| Title A → Z | `title_asc` | ☑ Enabled |
| Title Z → A | `title_desc` | ☑ Enabled |
| Author A → Z | `author_asc` | ☐ Disabled |
| Author Z → A | `author_desc` | ☐ Disabled |
| Featured: Yes first | `featured_desc` | ☐ Disabled |
| Featured: No first | `featured_asc` | ☐ Disabled |

Only enabled (checked) options appear in the visitor-facing dropdown. The user can drag to reorder the enabled options.

### Panel layout (sort mode)

```
┌──────────────────────────────────────────────┐
│ On Change                                     │
│ [📎 Articles] [applySort  ✕]                 │
├──────────────────────────────────────────────┤
│ Sort options (4 of 8 enabled)                 │
│                                               │
│ ☑ Date: Newest first              ≡  ← drag  │
│ ☑ Date: Oldest first              ≡          │
│ ☑ Title A → Z                     ≡          │
│ ☑ Title Z → A                     ≡          │
│ ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈  │
│ ☐ Author A → Z                              │
│ ☐ Author Z → A                              │
│ ☐ Featured: Yes first                        │
│ ☐ Featured: No first                         │
├──────────────────────────────────────────────┤
│ Show "Default" option: [✓]                    │
│ (resets to editor's sort setting)             │
├──────────────────────────────────────────────┤
│ Label: [Sort by ▾]            ← bindable      │
│ Placeholder: [Select...]      ← static        │
│ Disabled: [ ]                 ← bindable       │
│ Visibility: [ ]               ← bindable       │
└──────────────────────────────────────────────┘
```

### Baseline interaction

The editor user's design-time sort (set in the context config panel) is the **default**. The visitor's sort selection **replaces** the active sort (unlike filter, which layers):

- Editor sets: `sort: date desc` (in config panel)
- Visitor selects: "Title A → Z"
- Result at runtime: `sort: title asc` (replaces editor sort)
- Visitor selects "Default" option: reverts to `sort: date desc`

### Schema changes propagate

If the user adds a new field to the CMS collection, the sort options checklist updates automatically with two new entries (unchecked by default).

---

## Intent: Form Field (Deferred)

> Form support (section N in the use-cases map) is deferred. This section captures the direction for future alignment.

### Open questions

- **State management:** Where does intermediate form state live before submission? Options: Form context, Form container behavior, or direct collection write.
- **Pre-fill for update:** How does an "update" form pre-fill input values from the existing record?
- **Validation:** Schema-driven rules (required, min/max) are the baseline. Manual overrides possible. Schema wins at runtime (server-side enforcement).

### Expected pattern

A form Dropdown would bind `onChange` to a form-related function (e.g., `setFormField`). The `field` parameter would identify which collection field the input writes to. Options would derive from the field's enum values (same as filter derivation). The form container or form context would collect values from all inputs and expose a `submit()` action.

---

## Intent: Product Option Selector (Deferred)

> Product option support is deferred. This section captures the direction.

### Pattern: parameterize a sibling action

The Dropdown writes to a **context slot** (e.g., `selectedSize`). A sibling Button triggers an **action** that implicitly reads from that slot (e.g., `addToCart()` reads `selectedSize` + `selectedColor`). The context is the mediator.

```
Section "Product Detail"
└── Context: product (object)
    ├── Dropdown → onChange: selectOption(event, optionName: "size")
    │   Options: from product.sizeOptions (locked)
    ├── Dropdown → onChange: selectOption(event, optionName: "color")
    │   Options: from product.colorOptions (locked)
    └── Button → onClick: addToCart()
        (reads selectedSize + selectedColor from context state)
```

This generalizes to any pattern where an input qualifies another action without having standalone meaning.

---

## Options Behavior Summary

| Intent | Options source | User control | Derivation trigger |
|--------|---------------|-------------|-------------------|
| **Filter** (enum field) | Field's enum values | **Locked** | `field` parameter → schema enum |
| **Filter** (non-enum field) | User must bind or define | **Open** | Cannot auto-derive |
| **Sort** | All sortable fields × directions | **Curated** (check/uncheck + reorder) | Function type → schema fields |
| **Form** (deferred) | Field's enum values or static list | TBD | TBD |
| **Product option** (deferred) | Variant values from context | **Locked** | TBD |
| **No function bound** | User binds or defines | **Open** | No auto-derivation |

---

## Component Manifest

The Dropdown component manifest defines the component's properties and events. The manifest is the same regardless of intent — the panel adapts based on what `onChange` is bound to.

### Key properties

| Property | Type | Bindable | Description |
|----------|------|----------|-------------|
| `options` | arrayItems / textEnum | Yes | The list of choices displayed to the visitor |
| `value` | text | Yes (read) | The currently selected value |
| `label` | text | Yes | The label displayed above or beside the input |
| `placeholder` | text | No (static) | Placeholder text when nothing is selected |
| `disabled` | boolean | Yes | Whether the input is non-interactive |
| `required` | boolean | Yes | Whether a selection is mandatory (form mode) |

### Key events

| Event | Type | Description |
|-------|------|-------------|
| `onChange` | function (CHANGE domEvent) | Fired when the visitor selects an option. The bound function determines the component's role. |

---

## Relationship to Existing System

| Existing concept | How input binding uses it |
|-----------------|--------------------------|
| **Function binding** (Button onClick → addToCart) | Same mechanism: Dropdown onChange → applyFilter / applySort |
| **Function parameters** (sub-bindings) | Field, condition, direction etc. are parameters the user configures |
| **domEvent: CHANGE** (functions spec) | The event parameter auto-fills with the visitor's selection |
| **optional parameter** | Signals "no value is valid" → component renders "All" / "Default" reset option |
| **Context actions** (context.items with dataType: function) | applyFilter, applySort, selectOption are exposed as context actions |
| **Context config** (filter, sort, pageSize) | Editor's design-time settings remain the baseline; visitor input layers on top (filter) or replaces (sort) |
| **Auto-panel** | Not used for input components — custom panel adapts based on onChange binding |

---

## Open Questions

- **Cascading filters:** When multiple inputs filter the same context, how do dependent dropdowns update their available options? Does the context expose filtered-down distinct values per field?
- **Apply vs. immediate mode:** Is the apply mode (deferred filters with a commit button) a property of the context, the individual input, or a separate "Apply" button component?
- **Sort: two-input pattern:** Should the platform also support a separate field dropdown + direction toggle pattern (Pattern B), or only the combined dropdown (Pattern A) for the initial release?
- **Form context:** When forms are addressed, will a Form context be needed, or can the platform handle state collection via a Form container behavior?
- **Validation:** Where do validation rules live — schema, context manifest, component config, or all three? How do they compose?
- **Schema field metadata:** Should the context manifest declare which fields are sortable / filterable, or is every field assumed to be both?
- **Non-enum filter options:** For fields without predefined values (plain text, number), should the platform support a "distinct values" query to auto-populate filter options from the actual data?

---

*Document purpose: Define the input component binding model for the context binding platform. Covers architecture decisions, intent-driven behavior via function binding, options derivation, and panel UX for filter and sort use cases. Form and product option intents are deferred.*
