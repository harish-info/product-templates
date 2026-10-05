# Design Spec Generation Template

You are generating a Design Spec from raw input provided by a product manager or designer. The input may be a prototype codebase, Figma designs, screenshots, text notes, or a mix. Your job is to extract the visual states, user-facing content, interactions, and behavioral rules -- and produce a clean, structured spec that any platform team can implement from.

This document is the companion to the PRD. The PRD defines **what** to build and **why**. This spec defines **how it looks, what it says, and how it responds**.

## Input Handling

The input may come in different forms. Adapt your extraction approach:

- **Prototype codebase (Replit, CodeSandbox, etc.):** Read the component tree, state management, and rendered output. Extract the BEHAVIOR and CONTENT, not the implementation. Ignore framework APIs, library names, file structure, and code patterns. Focus on: what states exist, what strings are rendered, what conditionals control visibility, what user actions trigger transitions.
- **Figma designs or sketches:** Extract the screen structure, content hierarchy, states shown across frames/variants, and any annotations or notes from the designer. Ignore: exact spacing values, auto-layout settings, Figma component names, design tokens (including semantic token patterns like `color/surface/primary` or `spacing/md`). Focus on: what the user sees in each state, how elements are grouped, what varies between variants.
- **Screenshots or images:** Describe what is visible. Infer states from multiple screenshots showing the same screen in different conditions. Flag anything ambiguous with `[ASSUMPTION -- reason]`. Focus on: visible content, hierarchy, grouping, and any state differences between screenshots.
- **Text input (notes, brain dump, spec fragments):** Extract visual descriptions, state lists, copy/strings, and interaction rules. Ignore implementation suggestions unless they describe behavior.
- **Mixed input (combination of above):** Cross-reference sources. The PRD wins wherever it speaks: this spec follows its decisions on states, strings, rules and flows. Where the PRD is silent, prototype code is authoritative for states and conditional logic, Figma for visual hierarchy and grouping, and text notes for intent and business rules. Flag contradictions in the Unresolved Design Gaps section.

For any input type: if you cannot determine all states of a screen or component, list what you found and mark missing states with `[NEEDS DESIGN INPUT]`.

## Rules

### What belongs in a Design Spec

- Every distinct screen, view, overlay, or navigation component the user encounters
- Every state each screen can be in (default, empty, loading, error, success, offline, first-time)
- All user-facing strings in every language the product ships in (where the input has them): labels, headings, body text, helper text, confirmation messages, error messages, placeholder text, button labels, empty state copy
- Interaction rules: what the user can do on each screen, what it triggers, where it navigates
- Conditional visibility: what appears/disappears based on data, user type, feature flags, or prior actions
- Visual intent: what color meaning communicates (positive, negative, warning, neutral), what is prominent vs subtle, what is grouped together
- Content per item: for lists or cards, what data fields are shown and in what order, whether the count is fixed or variable, and any min/max constraints or overflow handling (truncation, scrolling, pagination)
- Confirmation and feedback: what the user sees after completing an action
- Transition intent: the intended feel of transitions between views (e.g., "slides up", "cross-fade", "instant switch") without specifying timing values
- Status systems: if the feature has statuses, enumerate all values with their visual treatment and associated copy
- Accessibility intent: how the design communicates meaning without relying solely on color (e.g., "status uses color + emoji + text label"), and any screen reader announcements for state changes
- Version variations: if the input describes A/B test configurations or feature flag variants, document each variant and what changes between them

### What does NOT belong in a Design Spec -- exclude even if present in the input

- Pixel dimensions: padding, margin, border radius, font sizes, icon sizes, elevation values, shadow specs, exact heights/widths
- Color codes: hex values, RGB, design token names in any format (e.g., gray-300, blue-600, color/surface/primary, spacing/md)
- Typography tokens: font family names, font weights, letter spacing, line height values
- Platform-specific components: @gorhom/bottom-sheet, Jetpack Compose components, SwiftUI views, React Native APIs
- Framework and library names: Reanimated, Expo Router, Koin, Coroutines, etc.
- File paths, component class names, function names from the prototype
- Layout implementation: flexbox properties, grid columns, auto-layout, constraint layouts
- Animation timing: duration, easing curves, spring configs, delay values
- API details: endpoint URLs, HTTP methods, JSON payloads, auth headers -- these belong in the PRD's behavioral contracts or backend spec

### Describing visual intent without pixel specs

Instead of exact values, describe visual meaning. Shape vocabulary (circular, pill-shaped, rounded) is permitted -- it describes form, not implementation.

| Don't write | Write instead |
|-------------|---------------|
| "64px circle, colored background, white text initials" | "Company logo (circular, brand color background, initials if no image)" |
| "borderRadius 5, detailStrong typography" | "Status badge (pill-shaped, color-coded by status)" |
| "16px padding, 1px border, gray-border" | "Separator line between cards" |
| "44px font size, centered, 8px top margin" | "Large emoji, centered, as the primary visual element" |
| "captionStrong, blue-600, 12px padding" | "Edit link, visually subtle, below the confirmation message" |
| "green-50 background, green-600 text" | "Positive treatment (green-tinted)" |

The goal: a developer on any platform can read the spec and build the right thing without referencing the prototype.

### Tone and style

- Write in plain language. A PM, designer, Android dev, iOS dev, web dev, and backend dev should all understand every sentence.
- No emoji in prose text. Emoji appear only in data tables when they are part of the product (e.g., status indicators shown to users). Use actual Unicode emoji characters, not platform-specific shortcodes (`:tada:`, `:star:`, etc.).
- Keep screen sections concise. If a Screen section exceeds 60 lines, consider whether sub-views should be promoted to their own Screen sections.

### The no-invention rule

Same as the PRD: **if information is not in the input, do not generate it.**

- Do not invent states that aren't shown or coded in the input
- Do not invent confirmation messages or error copy
- Do not invent interactions or gestures
- Do not assume a screen has an empty state unless one is shown or coded
- Do not invent counts for repeated elements (e.g., "3 skeleton cards") unless the input specifies them
- If a state is likely needed but not in the input, mark it `[NEEDS DESIGN INPUT]`

### Handling assumptions and contradictions

If the input is ambiguous (could be read multiple ways), pick the most likely interpretation and add `[ASSUMPTION -- reason]`. The PM or designer should verify assumptions before approval.

If the input contradicts itself (e.g., a string appears differently in two places, or a flow description conflicts with the prototype behavior), do NOT silently pick one. Preserve both in the Unresolved Design Gaps section with the interpretation used in the spec body.

### Handling strings and copy

User-facing strings are the most important part of this document. Capture them exactly as they appear in the input -- do not paraphrase, clean up grammar, or "improve" them. The PM or copywriter chose those words deliberately.

**Languages:** Capture each string in every language the product ships in, one column or line per language, headed by its language code, such as `String (en)`. The languages are the ones the team configured for generation; without that, the languages the input's strings come in. Where a string is missing in one of them, mark that one `[NEEDS TRANSLATION]`. If a string is identical in every language (brand names, numbers, technical terms), write it in each. The `<lang>` columns and lines below stand for one per language.

### Shared vs screen-specific content

If a pattern repeats across multiple screens (e.g., the same card layout, the same empty state structure, the same bottom sheet behavior), define it once in the Shared Patterns section and reference it from individual screens. Do not duplicate.

## Validation Checklist

The skill or assistant generating this design spec runs this checklist as its self-check before handing the document over, against the design spec as written and only on the sections it has. It is not part of the design spec: never write the checklist, its results or a summary of it into the document. A failure is fixed where the session already holds the answer, without inventing one, and otherwise reported in the summary to the PM.

### Content completeness
- Every screen listed in the inventory has a full section
- Every screen that fetches data has loading and error states documented (or marked [NEEDS DESIGN INPUT])
- Every screen has at least a default state and empty state documented (or empty state marked [NEEDS DESIGN INPUT])
- All user-facing strings are captured exactly as they appear in the input
- Strings are captured in every language the product ships in; missing translations marked [NEEDS TRANSLATION]
- Confirmation and feedback messages are listed for every state-changing action
- Conditional logic is explicit (no "as appropriate" or "when relevant")
- Entry points are listed for every screen
- Sort order is specified for every list (or marked [NEEDS DESIGN INPUT])
- Variable-length content has min/max/overflow behavior noted
- Status system is defined if the feature has one
- Shared patterns are defined and referenced, not duplicated
- Navigation map shows all connections between screens
- Version variations are documented if the input describes them
- Definitions section addresses visual vocabulary used across the spec

### Exclusion checks
- No pixel values (search for: px, dp, rem, pt, specific numbers followed by units)
- No color codes (search for: #, rgb, hex, token names like gray-300, blue-600, color/surface/primary)
- No typography tokens (search for: font family names, fontWeight, letterSpacing)
- No component or library names from the prototype
- No file paths or code references
- No API details (search for: endpoint, GET, POST, JSON, payload)
- No emoji shortcodes (search for: colon-delimited text like :tada: -- use Unicode emoji instead)

### Gap quality
- Every BLOCKER is something that would change the implementation if answered differently
- Every [NEEDS DESIGN INPUT] is something that affects what the user sees (not a technical detail)
- Every [ASSUMPTION] has reasoning that the PM/designer can verify
- No invented content -- every string, state, count, and interaction traces to the input
- Contradictions are surfaced in Unresolved Design Gaps, not hidden

---

## Output Format

Generate the Design Spec using the structure below. Repeat the "Screen" section for each distinct screen, overlay, or navigation component.

---

# [Feature Name] - Design Spec

**Date:** [date]
**Companion PRD:** [reference to the PRD this spec supports, or NEEDS INPUT]
**Input sources:** [list what was provided -- e.g., "Replit prototype codebase", "Figma file (URL)", "PM text notes", "3 screenshots"]

---

## Definitions

[Define visual vocabulary and terms that could be interpreted differently across platforms. Scan the input for terms that a PM, designer, Android dev, iOS dev, and web dev might understand differently.]

| Term | Definition |
|------|-----------|
| ... | ... |

[If genuinely no ambiguous terms exist, write "No ambiguous terms identified."]

---

## Screen Inventory

[List every screen, view, overlay, and navigation component covered in this spec. This is the table of contents.]

| # | Screen / View | Type | Phase | Notes |
|---|--------------|------|-------|-------|
| 1 | ... | Screen / Overlay / Navigation / Inline section | Phase 1 / Phase 2 | ... |

---

## Status System

[If the feature has a status or state system (e.g., application statuses, order states, verification levels), define it here once. Individual screens reference this table.]

[Skip this section if the feature has no status system.]

| Status | Visual treatment | Associated copy (<lang>) | Emoji / Icon |
|--------|-----------------|--------------------------|--------------|
| ... | Color meaning (e.g., "positive", "warning") | "Exact string" or [NEEDS TRANSLATION] | Unicode emoji |

**Accessibility note:** [How statuses are distinguishable without color alone -- e.g., "Each status uses color + emoji + text label, so color is not the sole differentiator."]

### Confirmation messages per status

[If status changes trigger feedback messages, list them here.]

| Status | Emoji | Message (<lang>) |
|--------|-------|------------------|
| ... | Unicode emoji | "Exact string from input" or [NEEDS TRANSLATION] |

---

## Shared Patterns

[Patterns that repeat across multiple screens. Define once, reference by name in screen sections.]

### [Pattern Name] (e.g., "Job Card", "Empty State", "Error State")

**Used in:** [list screens that use this pattern]

**Content:**
- [What elements are shown, in what order]
- [What data fields are displayed]
- [Any conditional elements]
- [If content is variable-length: min/max items, overflow handling]

**States:**
- [Default, selected, disabled, etc.]

**Interactions:**
- [What the user can do with this element]

---

## Screen: [Screen Name]

**Type:** Screen / Overlay / Bottom sheet / Navigation / Inline section
**Entry points:** [How the user gets here -- list all paths]
**Phase:** Phase 1 / Phase 2

### Purpose
[One sentence: what this screen lets the user do]

### States

[Enumerate every state this screen can be in. For each state, describe what the user sees. For screens that fetch data, loading and error states are required -- mark them [NEEDS DESIGN INPUT] if not in the input.]

#### Default state
- [What is visible, in order from top to bottom]
- [What data is shown per item if this is a list]
- [If a list: sort order, and whether it is paginated/infinite-scroll/fixed]

#### Empty state
- [What the user sees when there is no data]
- **Heading (<lang>):** "exact string" or [NEEDS TRANSLATION]
- **Helper text (<lang>):** "exact string" or [NEEDS TRANSLATION]
- **Icon/visual:** [description]

#### Loading state
- [What the user sees while data loads]

#### Error state
- [What the user sees when loading fails]
- **Heading (<lang>):** "exact string" or [NEEDS TRANSLATION]
- **Helper text (<lang>):** "exact string" or [NEEDS TRANSLATION]
- **Action:** [e.g., "Retry button"]

#### [Other states as needed -- offline, first-time, etc.]

### Content & Copy

[All user-facing strings on this screen, organized by context]

**Labels and headings:**

| Element | String (<lang>) |
|---------|-----------------|
| ... | "exact string" or [NEEDS TRANSLATION] |

**Button labels:**

| Button | String (<lang>) | Context |
|--------|-----------------|---------|
| ... | "exact string" or [NEEDS TRANSLATION] | [when shown] |

**Helper text and descriptions:**

| Context | String (<lang>) |
|---------|-----------------|
| ... | "exact string" or [NEEDS TRANSLATION] |

### Conditional Logic

[Rules that change what is shown based on data, user type, or state]

| Condition | Effect |
|-----------|--------|
| ... | Show / Hide / Change [element] |

### Interactions

[What the user can do on this screen and what happens]

| Action | Trigger | Result |
|--------|---------|--------|
| ... | Tap / Swipe / Long press / ... | [What happens -- navigation, state change, feedback] |

### Visual Intent

[How visual treatment communicates meaning -- no pixel values, no color codes]

- [What is visually prominent vs. subtle]
- [What color meaning communicates (positive/negative/warning/neutral, not color names)]
- [How elements are grouped or separated]
- [What the visual hierarchy communicates]

### Sub-views

[If this screen has distinct sub-views or modes (e.g., a bottom sheet with selection view and confirmation view), document each as a subsection here with the same structure: states, content, interactions.]

#### [Sub-view Name]
- ...

---

## Version Variations

[If the input describes A/B test configurations or feature flag variants, document what changes between each variant.]

[Skip this section if no version variations exist.]

| Version | [Dimension 1] | [Dimension 2] | ... | Use case |
|---------|--------------|--------------|-----|----------|
| ... | ... | ... | ... | [What this variant tests] |

---

## Navigation Map

[How screens connect to each other. Simple arrows showing user flow between screens in this spec.]

```
[Screen A] --action--> [Screen B] --action--> [Screen C]
                                   --action--> [Screen D]
```

---

## Unresolved Design Gaps

[Things that could not be determined from the input. Two severity levels:]
- `BLOCKER` -- cannot proceed to implementation without this answer (missing states, missing strings for primary flows, contradictions that change behavior)
- `NEEDS DESIGN INPUT` -- should be resolved but does not block development start

| # | Gap | Severity | Notes |
|---|-----|----------|-------|
| 1 | ... | BLOCKER / NEEDS DESIGN INPUT | ... |

### Contradictions in input

[If the input contradicts itself anywhere, list both statements here with the interpretation used in the spec.]

| Topic | Statement A | Statement B | Used in spec | Why |
|-------|------------|------------|--------------|-----|
| ... | ... | ... | ... | ... |

[If no contradictions found, write "No contradictions identified in the input."]
