---
name: reusing-component-templates
description: Use when duplicating, porting, or cloning an existing component so it can hold new content — "make the same panel for lesson 2", "use this as a template", "copy this section for the new topic", "same layout different data". Also use when filling an existing component's content slots with text taken from elsewhere in the repo or supplied by the user.
---

# Reusing Component Templates

## Overview

Copying a working component so it can carry new content looks mechanical. It is not.

Measured baseline — three agents, same task, **zero pressure applied to two of them**:

| Failure | Rate |
|---|---|
| Mutated strings they were only asked to move | 3/3 |
| Silently normalized punctuation or grammar | 3/3 |
| Wrote new prose into empty slots instead of asking | 3/3 |
| Pointed the new component at the old one's asset paths | 3/3 |
| Changed the template's structure unasked | 3/3 |
| Never handed over a list of files to create | 3/3 |

Not one was careless. Every mutation came with a defensible reason — a genuine grammar slip, a genuinely questionable statistic, a genuinely redundant duplicate file. **This failure is competence applied to the wrong axis.** A rule that only says "don't change things" loses this argument every time, because the agent is often right about the problem and wrong about its authority to fix it.

So the three laws each name what to do with the impulse instead of forbidding it:

1. **Text is transported, never authored.** Improvements go in the Findings report.
2. **Assets are re-declared, never inherited.** New names and paths go in the Asset manifest.
3. **Structure is copied, never redesigned.** Proposals go to the user before you write.

## When to Use

- "Make the same X for Y" · "use this as a template" · "copy this component for the new section"
- Porting a component across topics, lessons, tenants, locales, or routes
- Filling a template's content slots from a content file, another component, or a paste from the user

**Not for:** building a new component from scratch, refactoring a component in place, or restyling one. Those are ordinary work.

## Law 1 — Text is transported, never authored

Every string you did not write in this session is cargo. Move it byte-for-byte.

**Mutation includes all of these.** Each was observed in baseline:

| Kind | Observed |
|---|---|
| Rewriting a heading | `סיפורים אמיתיים מההיסטוריה` → `סיפורים אמיתיים על מפות` |
| Grammar "fix" | `לנחות בחוף הלא נכון` → `לנחות על החוף הלא נכון` |
| Adding a definite article | `מפת עולם` → `מפת העולם` |
| Punctuation normalization | `ישראל • שנות ה-2000` → `ישראל · שנות ה-2000` |
| Adding detail | `חופי נורמנדי · 1944` → `חופי נורמנדי · יוני 1944` |
| Reordering records | "so the 01–04 numbering means something" |
| Running a copy-editor over it | all four fields rewritten to second person |

A one-word edit is the most dangerous kind, because nobody catches it in review.

**Do not retype cargo — move it.** Extract the strings from the source programmatically, substitute them into the new file, then diff the result back against the source and confirm zero differences before you report. Retyping by hand is how single-character drift gets in, and it is the one failure your own review will not catch.

**Prose you may author:** `alt` text and image-generation briefs for assets you are declaring under Law 2. These describe artwork, not content. Everything else is cargo.

**Where new content comes from:** only a source the user points at — their message, a named content file, a named component. **A slot with no source stays empty with a `TODO` and goes in the Findings report.** Never fill it from your own knowledge, and never derive it from neighbouring text.

**The escape valve.** You will find real problems — a wrong figure, a typo, an unsourced claim. That instinct is correct and the finding is valuable. It goes in the Findings report. It does not go in the file. Report it, ship the original.

## Law 2 — Assets are re-declared, never inherited

Keyed to what is actually on disk. Check before you decide:

| Predicate | Action |
|---|---|
| A correct asset for this content exists on disk | Wire it up by its real path |
| It does not exist | Declare a new id + path following the codebase's naming convention, and add a row to the Asset manifest |

**Every asset path in the new component resolves inside the new component's own namespace.** A `topic-02` component referencing `/assets/.../topic01/...` is broken output, including for backgrounds, textures, and other "content-neutral" files. If a file genuinely should be shared, say so in Findings and leave the path pointing at the new namespace.

**"Exists on disk" means you looked.** One baseline agent opened the four existing images, saw they depicted a different subject, and refused to mislabel them — correct, and the standard. A filename that sounds right is not evidence.

## Law 3 — Structure is copied, never redesigned

Same fields, same record count, same order, same asset tiers, same variants.

Observed violations: deleting a preview-image tier from the type and every record; cutting an 8-asset order to 4 and substituting a crop; swapping a class for a nominally better one. Each was reported afterwards as a fait accompli.

**Wanting to change the structure is a reason to stop and ask, not a reason to change it.** Ask before writing, not after.

## REQUIRED output

Every template reuse ends with both blocks. A reuse without them is incomplete.

**1. Asset manifest** — one row per asset the new component references:

| Asset id | File to create | Destination folder | What it should show |
|---|---|---|---|

Rows for assets already on disk are marked `EXISTS — wired`. Rows for missing ones are the user's production order: exact filename, exact folder, and enough of a brief to generate from.

**2. Findings** — everything you noticed and did not act on:

- text problems left intact (quote the string, say what's wrong)
- factual claims a subject-matter reviewer should check, quoted
- slots left `TODO` for lack of a source
- structural changes you want and did not make
- files that arguably belong in a shared location
- assets loaded by a path that shows no placeholder when missing (raw CSS backgrounds, `<img src>`) — these fail silently, unlike a guarded asset component

An empty Findings section is a valid result. Say "none" rather than omitting it.

## Rationalizations — all observed verbatim

| Excuse | Reality |
|---|---|
| "no new claims, just distilled from the existing text" | Distillation is authorship. The slot has no source — it stays TODO. |
| "so the numbering means something" | Order is structure. Law 3. Ask. |
| "it also introduced a grammar slip which I corrected" | Correcting a slip is mutating cargo. Findings. |
| "the figure is unsourced, so I did not repeat it" | You are right, and it still ships. Findings. |
| "they're content-neutral paper grain" | Namespace, not content. Law 2. |
| "visually identical in that slot" | Then the user loses nothing by approving it first. |
| "a dedicated crop can be added later by changing two props" | Then don't remove it now. |
| "you mentioned house style, so I ran the copy editor" | House style applies to text you author. Cargo is not yours. |
| "reusing lesson 1's map is worse than a placeholder" | Correct — and a third option exists: declare the new asset. |
| "the demo is in 40 minutes" | Silent edits cost more review time than a manifest costs to read. |

## Red Flags — stop

- You are about to type a string that differs from its source by any character
- You are about to write prose for a field the user gave you no source for
- A path in the new file contains the old namespace
- You are about to invoke a copy-editing, fact-checking, or style agent **on text you are moving** (use it to *populate Findings*, never to edit)
- You catch yourself planning to mention a change "in the summary afterwards"
- The word "just", "slight", "minor", or "identical" is doing work in your reasoning

**All of these mean: restore the original, and move the observation to Findings.**

## Common Mistakes

- **Delivering the manifest as prose.** It was buried in a summary paragraph in 3/3 baselines. It is a table, or it did not happen.
- **Treating "consistency across the site" as licence to edit.** It is a reason to copy more exactly, not less.
- **Silent scope reduction.** Halving a deliverable and reporting it afterwards is not a decision you own.
- **Copying a diff instead of the file.** Read the whole source component before porting; the tiers you never noticed are the ones that get dropped.
