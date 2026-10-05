---
name: prd-plan
description: Use when a product manager, designer or engineer asks to create, write, generate or update a PRD (product requirements document) or a design spec for a feature, from notes, documents, a Figma design or a prototype. Triggers include "write a prd", "create prd", "update the prd", "spec out this feature", "design spec", "plan this feature".
---

# PRD plan

Turn a feature's raw inputs into a PRD and a design spec that follow the templates in this skill, then close the gaps with the PM one question at a time.

Find the templates next to this file: `templates/prd-template.md` and `templates/design-spec-template.md`, in the folder that holds this `SKILL.md`, not the working directory. They are the format and the rules: no invention, gap markers, platform neutrality, acceptance criteria IDs and the validation checklist. Follow them exactly.

## Adapt this

A team changes these lines, and nothing else, to fit its own setup.

- **Context:** a local folder or files the user names. Swap for a wiki folder, a docs export or anything else readable.
- **Output folder:** `./plans/<feature-name>/`, with `prd.md` and `design-spec.md`.
- **Platforms:** Android, iOS, Web, Backend, in that order, one row each in Platforms in Scope.
- **Languages:** the design spec captures strings in the languages the input's strings come in. Name fixed languages here, such as English and Norwegian.
- **Document layout:** edit the Output Format section of a template to add, drop or rename sections.
- **Design tools:** Figma through its MCP server when one is connected, otherwise screenshots or exported frames.
- **Privacy:** write people as roles ("the product manager", "a backend engineer"), never by name.

## Arguments

- `"<feature name>"`: quoted when it has spaces. Its folder name is the name in lowercase with hyphens.
- `--context <path>`: a folder or file; repeat for several.
- `--design <url-or-path>`: a Figma link, or screenshots; repeat for several.
- `--prototype <path-or-url>`: a local folder, a git URL or a hosted prototype link.
- `--prd` or `--spec`: only that document. Not both.

Without `--prd` or `--spec`, write both documents when a design or prototype was given or the user asks for a design spec, and only the PRD otherwise, offering the spec at the end. `--prd` never edits an existing design spec, and `--spec` never edits the PRD.

## Process

**1. Gather inputs.** Ask once, in one message, for whatever the arguments didn't give: the feature name, the context, the design and the prototype. For each design or prototype, also ask which platforms it shows. "None" is a valid answer for design and prototype: the feature is then a functional change. Don't draft before the feature name and context are known.

**2. Read.** Read the context files. Prefer decisions and constraints over discussion, and the newer statement where two disagree. Note which file each fact comes from.

From a prototype, take flows, states (empty, loading, error, success), data needs, rules and user-facing strings. Leave out frameworks, file structure, styling and endpoints. Clone a git URL into a temporary folder first.

From Figma, read each frame's design context and a screenshot. A link without a `node-id` names no frame: ask for a link to each screen that matters. Without a Figma connection, ask for screenshots or a description instead.

**3. Draft.** Generate the documents from the templates. Write them to the output folder straight away, and from then on make every change in those files and read them back, not from memory. Tell the user the folder in one line.

If `prd.md` already exists there, this is an update: keep every section the new input doesn't change word for word, keep every acceptance criterion's ID, move a removed criterion to Retired and never reuse its ID, and keep the design spec in line with the PRD.

`--spec` reads the PRD in the output folder when there is one, and follows it. Without one, say so in the spec's first line.

**4. Grill session (PRD).** Skip with `--spec`. Go through these in order. Each step is one question per message, and you wait for the answer before the next.

1. **From the context.** Fill each gap marker the context answers, and report them in one list, each with the file it came from. The documents carry no source comments. The Tracking gap is never filled this way.
2. **Design or prototype against the PRD.** For each place they disagree (a state, a string, a rule, a flow, a platform), show what each side says and let the PM decide. The PRD wins: a skipped one keeps the PRD's answer and goes under Open Questions > Conflicts.
3. **Blockers.** For each `[BLOCKER -- NEEDS INPUT]`: name the section, say what it needs, and give a recommended answer when the context suggests one.
4. **Tracking.** Always ask: "Which analytics events does this feature add or change, and where is your tracking plan? Or confirm it has no tracking." Events the context mentions are suggestions for the PM to confirm, never the answer.
5. **Regular gaps.** Say how many `[NEEDS INPUT]` remain and ask whether to go through them now.
6. **Assumptions.** List every `[ASSUMPTION -- ...]` in one message to confirm or correct.

When an open item waits on a known event, such as a legal review, add `resolve by YYYY-MM-DD` after its marker on the same line.

When the PM says skip, or isn't available, leave the marker in place and move on.

**5. Keep the design spec in line.** When an answer changed scope, platforms, flows, states, strings or conditions, and a design spec exists, regenerate the parts it touches. With `--prd`, or when the PM declines, leave the spec as it is and the summary calls it stale.

**6. Self-check.** Run each template's Validation Checklist against its document. Fix what the session already holds the answer to, without inventing; list the rest in the summary. Never write the checklist into a document.

For a new PRD, the Revision history line is `- v1, <today>: first plan`. An update adds `- v<N>, <today>: <what changed>. <AC changes by ID>.` and never edits an older line.

**7. Summary.** End with:

```
<feature name>
  Files: <paths>
  Acceptance criteria: <N, or added / changed / retired IDs>
  Resolved in the grill: <N from context, N from the PM>
  Blockers left: <N, listed>
  Gaps left: <N>
  Tracking: <events and plan link | no tracking, confirmed | not answered: PRD not ready>
  Design spec: <in line with the PRD | stale | not written>
  Self-check: <passed | failures, one per line>
```

When the PM asks for a reader test: write 5 to 8 questions an engineer would ask first, give only the documents and the questions to a fresh subagent, and report what it couldn't answer from the documents alone.

## Common mistakes

| Mistake | Instead |
|---|---|
| Sending all the gaps as one long list | One question per message, blockers first, with a recommended answer |
| Filling a gap with a sensible default | Leave the marker; the PM decides what's standard |
| Answering Tracking from the context | Ask the PM every time |
| Drafting before asking about a design or prototype | Ask in step 1, accept "none" |
| Reporting both documents ready after the grill changed scope | Update the design spec, or call it stale |
| Reading templates from the working directory | Read them from this skill's folder |
