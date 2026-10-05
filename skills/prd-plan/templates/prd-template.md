# PRD Generation Template

You are generating a Product Requirements Document from raw input provided by a product manager. The input may be messy, verbose, or mixed with implementation details. Your job is to extract the product requirements and produce a clean, structured PRD.

## Input Handling

The PM's input may come in different forms. Adapt your extraction approach:

- **Written document (markdown, notes, brain dump):** Extract directly. This is the most common case.
- **Prototype or codebase reference:** Extract the behavior and user flows, not the implementation. Ignore file paths, tech stack, component names, and code patterns.
- **Figma or design reference:** Extract the user flows, states, and content. Ignore layout, spacing, colors, and design tokens.
- **Slack messages or transcripts:** Extract decisions, requirements, and open questions. Ignore conversational noise, reactions, and off-topic messages.
- **Mixed input (combination of above):** Treat each source type with its own rules. Cross-reference for consistency and flag contradictions.

For any input type: if the input is too sparse to fill a required section, mark it with the appropriate gap severity. If the input is extremely large (1000+ lines), prioritize extraction in this order: constraints > non-goals > explicit decisions > user flows > supporting detail.

## Rules

### What belongs in a PRD
- Problem and why it matters now
- What exists today (current state of the product)
- Who it's for and who it's not for
- What we're building, what we're not building, and why
- How users interact with it (narrative flows with decision points, not UI specs)
- How this connects to existing systems and services
- Behavioral contracts for operations the system performs (what goes in, what comes out, what can go wrong)
- What data it needs and what constraints exist
- How we know it's done (testable acceptance criteria -- observable behavior, not adoption metrics)
- How we measure success and when to kill it
- What could go wrong and how to handle it
- What's unresolved, including contradictions in the input
- Shared vocabulary for ambiguous terms
- Which platforms are in scope for each phase
- Which analytics events it adds or changes and where the team's tracking plan is, or that it has none, as the PM gave them

### What does NOT belong in a PRD -- exclude even if present in the input
- UI specifications: pixel values, padding, margins, border radius, font sizes, color hex codes, typography tokens, component dimensions, animation timings, shadow specs
- Design system references: Figma node IDs, design component names
- API syntax: endpoint URLs, HTTP methods, auth headers, JSON request/response payloads, SQL schemas
- Library or framework names: no @gorhom/bottom-sheet, react-native-reanimated, Jetpack Compose, SwiftUI, PostgreSQL, etc.
- Platform-specific implementation: no Android/iOS/web/backend code patterns, architecture patterns, state management approach
- Prototype details: file paths, tech stack, mock data structure, navigation routes, component hierarchy
- Project management: week-by-week timelines, sprint plans, who-does-what schedules
- Screenshot placeholders

### The no-invention rule
**If information is not in the input, do not generate it.** This applies to everything:
- Do not invent rollout strategies (e.g., "10% soft launch") unless the PM specified one
- Do not invent launch gates, instrumentation plans, or operational milestones
- Do not invent user research, evidence, or competitive analysis
- Do not invent performance targets or SLAs
- Do not fill gaps with "reasonable defaults" or "standard practice"
- If a section feels incomplete without invented content, mark it [NEEDS INPUT] and move on

The PM decides what's standard for their team. A PRD with honest gaps is more useful than one with plausible-looking fiction.

### Tone and style
- Write in plain language. A PM, designer, Android dev, iOS dev, web dev, and backend dev should all understand every sentence without domain translation.
- Use concrete language, not abstract. "User sees a list of jobs they applied to" not "The system surfaces relevant application entities."
- Keep sections tight. If a section exceeds 15 lines, it's too long -- compress or split.
- No emojis in body text. Emojis may appear in data tables only if they are part of the product (e.g., status indicators shown to users).
- No marketing language. No "delightful experience" or "seamless integration."

### Handling gaps
There are two severity levels for gaps:
- `[BLOCKER -- NEEDS INPUT]` -- PRD cannot be approved without this. Used for: missing problem statement, missing goals, missing target users, missing acceptance criteria for primary KRs, missing constraints that would change the solution.
- `[NEEDS INPUT]` -- Should be filled before development starts but does not block PRD approval. Used for: missing evidence, missing performance targets, missing rollback criteria, current state details, integration details.

For sparse inputs (only a few paragraphs of context), only mark BLOCKER for problem statement, goals, and target users. Everything else defaults to regular [NEEDS INPUT].

If the input is ambiguous (could be read multiple ways), pick the most likely interpretation and add `[ASSUMPTION -- reason]`. The PM should verify assumptions before approval.

### Handling contradictions
If the input contradicts itself (e.g., two different default statuses, conflicting scope statements), do NOT silently pick one. Preserve both statements in the Open Questions > Conflicts subsection, note which interpretation you used in the PRD body, and mark it `[ASSUMPTION -- conflict between X and Y, chose X because Z]`.

### Platform neutrality
- Describe behavior, not implementation. "User taps a job card and sees status options" not "Bottom sheet opens with @gorhom/bottom-sheet at 85% snap point."
- Describe data needs, not schemas. "We need to track which jobs a user applied to, when, and their self-reported status" not "CREATE TABLE job_status (ad_id INT, user_id INT...)."
- Describe flows, not screens. Each platform will implement the same flow differently -- the PRD defines what happens, not how it looks.
- Name concepts, not components. "Status selector" not "StatusPickerBottomSheet." "Job card" not "JobCardApplicationsVariant."

## Validation Checklist

The skill or assistant generating this PRD runs this checklist as its self-check before handing the document over, against the PRD as written and only on the sections it has. It is not part of the PRD: never write the checklist, its results or a summary of it into the document. A failure is fixed where the session already holds the answer, without inventing one, and otherwise reported in the summary to the PM.

### Content completeness
- Problem statement describes user pain, not a feature
- Current state section describes what exists today
- At least one measurable goal (OKR/KPI) is defined
- Target users are specific (not "all users")
- Platforms in scope are explicitly listed, and every phase cell is `Yes`, `No` or `[NEEDS INPUT]`
- Non-goals section contains only explicitly stated or strongly implied rejections (no inferred non-goals)
- In-scope items are prioritized (must/should/could)
- Every in-scope item ends with its phase and area code, such as `(phase 1, SAVE)`, matching the acceptance criteria that cover it
- User flows include decision points where behavior branches
- Integration points are mapped
- Behavioral contracts capture operations with inputs, outputs, and error behavior
- Acceptance criteria are testable and contain only observable behavior (no adoption metrics)
- Phases are consistent: every phase a platform marks Yes in Platforms in Scope has its own acceptance criteria, and no criterion requires something the Scope section puts in another phase
- Every acceptance criterion starts with an `AC-<AREA>-NN` ID, no ID appears twice, and no retired ID is used again
- When updating a plan, every criterion of the previous version is still present with its ID or listed under Retired
- Tracking holds the PM's answer: the events and tracking plan link they gave, with nothing invented, or no tracking as they confirmed
- Dependencies show build order
- Success metrics include rollback criteria
- Open questions are grouped by urgency
- Revision history has a line for this version, and every older line exactly as it was
- Definitions section addresses ambiguous terms from the input

### Exclusion checks
- No UI specs leaked in (search for: px, rem, dp, hex colors, component names, animation timings)
- No API syntax leaked in (search for: endpoint URLs, JSON payloads, HTTP methods, SQL)
- No library/framework names appear anywhere
- No week-by-week timelines or sprint plans
- No prototype or codebase references
- No invented content (search for rollout percentages, launch gates, or metrics not in input)

### Gap quality
- Every [BLOCKER -- NEEDS INPUT] is something that would change the solution if answered differently
- Every [NEEDS INPUT] is something the PM must answer, not something extractable from the input
- Every [ASSUMPTION] has reasoning that the PM can verify
- No BLOCKER items are left unaddressed in sections that have sufficient input data
- Contradictions are surfaced in Open Questions, not hidden

---

## Output Format

Generate the PRD using exactly the structure below. Do not add, remove, or rename sections.

---

# [Feature Name] - Product Requirements Document

**Date:** [date]
**Owner:** [extract from input or mark NEEDS INPUT]
**Deadline:** [extract from input or mark NEEDS INPUT]

---

## Definitions

[Define terms that the PM uses inconsistently, or that could mean different things to PM, design, and engineering.]
[Scan the input for overloaded terms -- words used with multiple meanings, synonyms used interchangeably, or domain terms that non-specialists would misunderstand.]

| Term | Definition |
|------|-----------|
| ... | ... |

[This section is required whenever the input contains ambiguous terms. If genuinely no ambiguous terms exist, write "No ambiguous terms identified."]

---

## Problem Statement

[What problem are users facing today? Describe the pain, not the solution. Include:]

- What users struggle with or cannot do
- Current workaround (if any)
- Why this matters now (business trigger, OKR cycle, competitive pressure, user feedback)

**Evidence:**
[List supporting evidence from the input. If the PM didn't provide evidence, write:]
[BLOCKER -- NEEDS INPUT] No user research, analytics, or support data cited. This section should reference interviews, surveys, support ticket themes, or usage data that validates the problem. Without evidence, the team cannot evaluate whether the solution fits the actual problem.

---

## Current State

[Describe what the product does today in the area this feature touches.]
[What exists? What services, data, and user-facing features are already live?]
[Engineers and agents need to know the starting point -- what they're building on top of, not just what they're building.]

[Extract from mentions of "existing" data sources, APIs, services, or features in the input. If the input doesn't describe current state at all, write:]
[NEEDS INPUT] No current product state described. Engineering needs to know what exists today in this area before starting implementation.

---

## Goals & OKR Alignment

[Map to team/company objectives. Every PRD must trace to a measurable goal.]

| Key Result | Baseline | Target | Status | Delivery |
|------------|----------|--------|--------|----------|
| ... | ... | ... | ... | ... |

[If no OKRs in input, mark BLOCKER -- NEEDS INPUT: "PRD must tie to at least one measurable business objective."]

---

## Target Users

[For each segment:]
- Who they are (role, behavior pattern, context)
- Why this problem is acute for them
- How their behavior shapes the solution

[Also state who this is NOT for and why. This prevents engineers from over-generalizing.]

---

## Solution Overview

[1-3 paragraphs max. What are we building at a high level?]
[If phased, summarize each phase in one sentence and note the long-term vision if the input describes one.]
[Focus on user experience, not technical architecture.]

---

## Platforms in Scope

[Which platforms ship in which phase? Be explicit. One row per platform the team ships on, in a fixed order; the rows below are for android, ios, web and backend, with web split by the input into desktop and mobile.]

| Platform | Phase 1 | Phase 2 | Notes |
|----------|---------|---------|-------|
| Android | Yes/No | Yes/No | ... |
| iOS | Yes/No | Yes/No | ... |
| Web (desktop) | Yes/No | Yes/No | ... |
| Web (mobile) | Yes/No | Yes/No | ... |
| Backend | Yes/No | Yes/No | ... |

[Add or remove rows as needed -- if the input distinguishes additional surfaces (e.g., tablet, TV, watch), add them.]

[Each phase cell is exactly `Yes`, `No` or `[NEEDS INPUT]`, so that people and tools reading the PRD can rely on it. A qualification, such as verification only or no new work, goes in Notes, and the cell still says whether the platform has work in that phase, or `[NEEDS INPUT]` until the PM decides. One column per phase the plan has: a plan with one phase has no Phase 2 column.]

[If the input doesn't specify platforms, mark NEEDS INPUT. Do not assume "all platforms" by default.]

---

## Scope

### In scope (prioritized)

[Bulleted list grouped by priority. Be concrete.]
[Use MoSCoW: Must have, Should have, Could have.]
[If Phase 1 slips, the team cuts from the bottom up.]
[Only assign priority based on explicit signals in the input (OKR targets, deadlines, "critical", "nice-to-have"). If priority is unclear, mark items as Should have with [ASSUMPTION].]
[Each item ends with its phase and the area code of the acceptance criteria that cover it, in parentheses: `(phase 1, SAVE)`. An item with criteria in more than one phase or area names each: `(phases 1 and 2, SAVE)`, `(phase 1, SAVE, ALERT)`. Whoever writes tickets from the PRD takes each story's criteria and priority from these. A part not known yet is `[NEEDS INPUT]`, such as `(phase 1, [NEEDS INPUT])` for an item no criterion covers.]

**Must have (launch blockers):**
- ... (phase 1, AREA)

**Should have (expected but cuttable under pressure):**
- ... (phase 1, AREA)

**Could have (included if time permits):**
- ... (phase 2, AREA)

### Out of scope (deferred)
[Things planned for later. Include target phase/quarter so teams don't build them early.]

### Non-goals (decided against)

[CRITICAL for agents. List things the PM has actively decided NOT to do, with reasoning.]
[Format: "We will NOT [thing] because [reason]."]

[Only include non-goals that the input explicitly states or strongly implies through direct language (e.g., "not prioritized because...", "decided against...", "privacy-first means no..."). Do NOT infer non-goals from omission or silence -- if something is simply not mentioned, it is unaddressed, not rejected. List unaddressed areas as [NEEDS INPUT] in Open Questions instead.]

[If the input provides no explicit non-goals, write: "[NEEDS INPUT] No non-goals identified in the input. The PM should list capabilities that were considered and rejected, so that dev agents do not build them."]

---

## User Flows

[Describe the golden path and key alternative paths.]
[Use platform-neutral language. Describe what the user does and what happens, not how it's rendered.]
[Include decision points explicitly -- where the flow branches based on state or user choice.]
[Include error/edge paths only if they affect product behavior, not UI recovery.]

### Primary Flow: [Name]

1. User does X
2. System shows Y
3. **Decision point:** If [condition A], go to step 4. If [condition B], go to step 6.
4. ...

### Alternative Flow: [Name]

1. ...

---

## Integration Points

[How does this feature connect to existing systems?]
[List services, data stores, and features that this feature reads from, writes to, or modifies.]
[This is NOT an API spec -- it's a map of what's touched so each platform team knows the integration surface.]

| System | Relationship | Notes |
|--------|-------------|-------|
| ... | Reads from / Writes to / Creates new | ... |

[Extract from mentions of existing services, data sources, or APIs in the input. If unclear, mark NEEDS INPUT.]

---

## Behavioral Contracts

[Describe the operations this feature performs at a behavioral level.]
[This is NOT an API spec -- no endpoint URLs, no HTTP methods, no JSON payloads.]
[Each operation describes: what triggers it, what goes in, what comes out, key rules, and error behavior.]

| Operation | Trigger | Input | Output | Rules | Error behavior |
|-----------|---------|-------|--------|-------|----------------|
| ... | ... | ... | ... | ... | ... |

[Extract from any API definitions, technical architecture, or data flow descriptions in the input. Strip the syntax, keep the semantics.]

---

## Data & Constraints

### Data sources
[What data does this feature need? What exists vs what's new?]
[Describe at concept level: "Application history from two existing sources: submitted applications and apply click tracking. Deduplicated by user + job." Not schemas or SQL.]

### Constraints
[Hard limits that shape the solution. Things the engineering team cannot change.]

Examples of constraints to look for in input:
- Data access limitations (can't access X system, no partner-side data)
- Privacy/legal requirements (data visible only to user, GDPR, retention rules)
- Platform constraints (must work offline, must support X OS version)
- Business constraints (cannot change existing Y flow, must reuse Z service)

---

## Acceptance Criteria

Each criterion has a permanent ID. Tickets, reviews and later versions of this plan refer to a criterion by its ID, so the ID stays when the wording changes.

[Testable criteria grouped by goal or feature area.]
[Each criterion must describe observable behavior that an engineer can verify by testing one specific thing.]
[Do NOT include adoption metrics here (e.g., "100 users update status"). Those belong in Success Metrics.]
[Do not write vague criteria like "Works well" or "Good performance."]
[If phased, each phase must have its own acceptance criteria section.]
[IDs: each criterion starts with `AC-<AREA>-NN`. AREA is a short uppercase code for its feature area, 2 to 8 letters, shown after the area heading, such as SAVE for "Saved searches". NN counts up from 01 within the area, in two digits, and never restarts across phases.]
[When updating a plan: a criterion that still states the same requirement keeps its ID, even reworded or moved to another phase. A new criterion takes the next number after the highest ever used in its area, retired ones included. A removed criterion moves to Retired with the version that removed it and its last wording; its ID is never used again. A PRD whose criteria have no IDs gets them in the order they appear.]
[Plain bullets, never checkboxes: the PRD states what done means, it doesn't track progress.]

### Phase 1: [Name]

#### [Feature Area / KR] (AREA)

- AC-AREA-01: ...
- AC-AREA-02: ...

### Phase 2: [Name] (if applicable)

#### [Feature Area / KR] (AREA)

- AC-AREA-03: ...

### Retired

[Only when a criterion has been removed. Omit this heading otherwise.]

- AC-AREA-NN (removed in version N): [its last wording]

---

## Tracking

[The analytics events this feature adds or changes, and a link to the team's tracking plan, only as the PM gave them when asked. Never invent an event, a property or a link, even when the input suggests one: the PM confirms each.]
[When the PM states the feature has no tracking, this section is that one line: No tracking for this feature (confirmed by the PM during planning).]
[Until the PM answers, this section is `[BLOCKER -- NEEDS INPUT] Analytics events and the tracking plan link, or confirmation that this feature has no tracking.` The PRD isn't handed over with it unanswered.]

**Tracking plan:** [link]

| Event | Fires when | Platforms | Notes |
|-------|------------|-----------|-------|
| ... | ... | ... | ... |

---

## Dependencies

[What blocks what? Show the build order.]
[Use a simple chain or list format. Engineers and agents need to know what to build first.]

```
[prerequisite] --> [next step] --> [next step] --> [launch]
```

[If the input contains a week-by-week timeline, extract only the dependency relationships, discard the dates.]
[Flag any dependency where the owner or timeline is unclear with NEEDS INPUT.]

---

## Edge Cases

[Known edge cases and how to handle them.]
[Focus on data integrity and business logic, not UI error states.]
[Format: scenario, then handling rule.]

---

## Success Metrics

### Delivery (did we ship it?)

| Metric | Target | Tracking |
|--------|--------|----------|
| ... | ... | ... |

### Adoption (are users engaging?)

| Metric | Target |
|--------|--------|
| ... | ... |

[Adoption metrics and delivery targets belong here, NOT in Acceptance Criteria.]

### Rollback criteria
[When do we kill or pause this feature? Be specific with thresholds and timeframes.]
[Example: "Roll back if status update rate < 5% after 2 weeks of full rollout."]
[If not in input, mark NEEDS INPUT -- every shipped feature should have kill criteria. Do not invent thresholds.]

---

## Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| ... | ... | ... |

[Extract from input. Also flag risks the PM may have missed based on the constraints, edge cases, and dependencies sections.]

---

## Open Questions

[Unresolved decisions. Number them for reference.]
[If the input has questions scattered throughout (inline TODOs, "TBD", "need to decide"), consolidate them here.]
[Group by urgency.]

### Blocks development
1. **[Topic]:** [Question]

### Can be resolved during development
2. **[Topic]:** [Question]

### Conflicts in input
[If the input contradicts itself anywhere, list both statements here with the interpretation used in the PRD.]

| Topic | Statement A | Statement B | Used in PRD | Why |
|-------|------------|------------|-------------|-----|
| ... | ... | ... | ... | ... |

[If no conflicts found, write "No contradictions identified in the input."]

---

## Revision history

[One line per version, oldest first, written by whoever generates the PRD. A new plan writes `- v1, [date]: first plan`. Every update adds its own line at the end and never rewrites an older one:]
[`- v<N>, <YYYY-MM-DD>: <one-line summary>. <the AC changes by ID: added, changed, retired>. Previous: <link to the previous version, when one is kept>`]
[Version control or the document's own history keeps the old versions: never copy one here.]

- v1, [date]: first plan
