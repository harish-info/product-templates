# Product Templates

Templates for writing a PRD and a design spec, and a skill that writes both for you from your notes, designs and prototypes.

## What's inside

| File | What it is |
|---|---|
| [prd-template.md](skills/prd-plan/templates/prd-template.md) | PRD template: problem, goals, users, scope, flows, acceptance criteria, tracking, risks |
| [design-spec-template.md](skills/prd-plan/templates/design-spec-template.md) | Design spec template: screens, states, strings, interactions |
| [prd-plan](skills/prd-plan/SKILL.md) | A Claude Code and Codex skill that generates both documents |

The templates keep product separate from implementation (no pixel values, framework names or API syntax). They also mark what the input doesn't say instead of guessing it: `[BLOCKER -- NEEDS INPUT]`, `[NEEDS INPUT]` or `[ASSUMPTION -- reason]`.

## Use a template

Copy a template into Claude, ChatGPT or any assistant, add your notes, and ask for the PRD or design spec.

## Use the skill

The skill reads your notes and any Figma link or prototype, writes the documents to `./plans/<feature-name>/`, then asks you about the gaps one question at a time.

Install in Claude Code:

```
/plugin marketplace add harish-info/product-templates
/plugin install product-templates@product-templates
```

Install in Codex: copy `skills/prd-plan` into `~/.agents/skills/` and restart.

Run it in Claude Code:

```
/product-templates:prd-plan "Saved search alerts" --context ./notes --design <figma-link> --prototype <path-or-url>
```

In Codex, mention `$prd-plan` with the same options, or just ask: "write a PRD for saved search alerts from ./notes".

To fit your team, edit the **Adapt this** section at the top of [SKILL.md](skills/prd-plan/SKILL.md).
