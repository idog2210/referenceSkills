---
name: checking-design-fidelity
description: Use when writing, editing, or reviewing styled UI (JSX/CSS/className/style) in a codebase that has an established design system — design tokens, a Tailwind/theme config, a written spec or mockup, or an existing reference component other UI already matches. Also use whenever asked to check a component, section, or page for visual consistency with the rest of the app.
---

# Checking Design Fidelity

## Overview

UI drifts when values are approximated instead of reused: a "close enough" `text-gray-500` instead of the project's real muted-text token, an extra heading tier that isn't in the reference, a documentation label typed in as if it were a class name. This skill is a repeatable procedure — **Locate → Map → Verify → Flag** — for keeping any styled edit inside a codebase's actual, already-established design system instead of inventing new-but-plausible values.

This is a generic/methodology skill: it has no hardcoded colors or class names, because those differ per project. If the current project already has its own concrete version of this skill (check for a project-scoped skill, e.g. `.claude/skills/checking-design-fidelity/`), that one wins — it has real tables. Use this version to build that kind of table when one doesn't exist yet, or in any project that doesn't have one.

## When to use

- Adding or editing any element with visual styling: heading, body text, button, card, badge, spacing, color.
- Asked to "check X matches the design" / "keep this consistent" / review a section for drift.
- The project has any of: a design spec/mockup doc, a token or theme config file (Tailwind config, CSS custom properties, a `design-tokens.json`, a theme object), or a component other similar components are clearly meant to match.

## Step 1 — Locate the source of truth

Look, in this order, for:

1. **A design spec or style-guide doc** — `design/*.md`, `STYLEGUIDE.md`, `docs/design-system.md`, Figma-exported specs, a mockup image with an accompanying values doc.
2. **A token/config file** — `tailwind.config.*`, a `theme.ts`/`theme.json`, CSS custom properties under `:root` in a global stylesheet, a `design-tokens.json`.
3. **An established reference component** — grep for a repeated class pattern across sibling components, or check git history for commits like "unify X to Y reference" / "align X with Y" — that phrasing is a strong signal a canonical pattern already exists and other files have drifted from it before.

If none of these exist, there is no established system yet to check against — building one is a separate task (design work), not this skill's job.

## Step 2 — Map: build (or reuse) a quick-reference table

For the area you're touching, gather real, verbatim values:

- Heading / sub-heading / body: exact font-size, weight, color class or token, per tier.
- Buttons: each variant's exact classes.
- Colors: exact class or CSS-variable name, not a paraphrase.
- Spacing / radius / shadow for the component family you're editing.

**The critical trap:** spec/mockup docs frequently label colors and sizes with documentation-style names (`--ink-900`, `"Heading/Large"`, `Ink 900`) that are *not* the literal class or variable name used in code. Never type a doc label directly into a `className`. Resolve it: grep the token/config file for the *hex value* or the *semantic role* the doc describes, and use whatever real name that file actually defines. If the doc says `--ink-900 #38432E` and the config defines `colors.olive.ink = '#38432E'`, the real class is `text-olive-ink` — not `text-ink-900`.

If a table like this was already built earlier in this project (by a prior pass of this skill, or as a project-scoped skill), reuse it rather than re-deriving it each time.

## Step 3 — Verify before finishing

1. **Identify which reference applies** — which spec section, config tokens, or reference component this element belongs to.
2. **Copy values verbatim** from the table you built, or from an existing sibling file — never reconstruct a class/token name from memory or from a doc label.
3. **Grep to confirm before writing** anything you're not 100% sure exists: search the token/config file for the class or variable name. Not found → it doesn't exist, don't write it.
4. **Match the established tier count exactly.** If the reference is heading → sub-heading → body (3 tiers), don't add a 4th micro-label "for clarity." If the reference is flat/uncolored across a category of elements, don't color-code them "to help" — that is itself a drift pattern real projects have had to spend multiple commits undoing.
5. **Diff your finished classes against the reference row/file** before calling the change done.
6. **Missing value → stop, don't invent. Ask, quoting the source precisely.** If the exact color/spacing/size/tier you need isn't in the spec, tokens, or any reference file — a heading type you don't know how to classify, for instance — don't substitute a "close enough" default. Ask the user in your reply message. The question must quote **the exact first 3 words of the source text you're unsure about, verbatim, and no more** (not the full sentence, not a paraphrase, not your own summary of it) — then state what's undetermined. Quoting the precise source, rather than restating it in your own words, keeps the question anchored to what's actually ambiguous instead of to your interpretation of it. Alternatively, record it as a documented assumption if the project has a place for that and the user says to proceed with a guess (an `assumptions.md`, a comment, a linked issue) — but don't skip straight to that without asking first.

   Example: an element reads `"Section overview —"` and it's unclear whether it maps to your table's Heading row or its Sub-heading row. Ask: `"Section overview —" — is this a section heading or an in-body sub-heading?` — not the full text, and not a reworded description of it.

## Common mistakes

| Mistake | Reality |
|---|---|
| Typing a spec doc's documentation label in as a literal class name | Doc labels are not implemented tokens — resolve to the real name in the config/token file first. |
| Substituting a generic "close enough" value (`text-gray-500`, `font-semibold`, `16px`) for the project's actual token | Reuse the exact token already defined — a plausible-looking substitute is still drift, and someone will eventually have to unify it back. |
| Adding an extra hierarchy tier that isn't in the reference | Match the reference's tier count exactly. |
| Color-coding or otherwise varying elements that the reference treats uniformly | If the established pattern is uniform, stay uniform — don't add variation "for clarity" unless asked. |
| Treating visual similarity as verification | "Looks about right" isn't the check — diff against the actual reference values before finishing. |
| Reusing a token from an unrelated part of the design system (e.g. a marketing-page token inside an app-internal component, or vice versa) | Established systems are often scoped (marketing vs. app, light vs. dark surface) — stay inside the scope the element actually belongs to. |
| Guessing at an uncovered case (an unfamiliar heading type, color role, or component) instead of asking | Ask in the reply message, quoting only the exact first 3 words of the source text — no full quote, no paraphrase. |

## Building a project-scoped version

When a project has real, concrete values worth capturing once (a spec doc, a token file, a reference component with exact classes), it's worth writing them down as a project skill instead of re-deriving them on every task: create `.claude/skills/checking-design-fidelity/SKILL.md` in that repo, following this same Locate → Map → Verify structure, but with the project's actual tables filled in (real class names, real hex-to-token mappings, the exact reference component's classes). That project-scoped skill then takes priority over this generic one whenever you're working in that repo.
