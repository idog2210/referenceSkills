---
name: reference-to-ui-exact
description: Use when the user runs /reference-to-ui-exact to rebuild an existing screen or component from a reference image with measured visual fidelity inside the real project, gated by an audit-only phase and requiring explicit written approval before any code change.
disable-model-invocation: true
user-invocable: true
argument-hint: "[target route/component] [reference image path]"
---

# Reference to UI Exact

This is a mandatory-execution skill. Every instruction sentence, numbered step, required table, verification step, and stop condition in this file is **MANDATORY**, never advisory, unless explicitly labeled `OPTIONAL`. This skill deliberately uses `MUST`, `MUST NOT`, `STOP`, `BLOCKED`, and `FAIL`. Treat each as a hard requirement. Words like "should", "preferably", "try", "consider", "when convenient", or "if possible" MUST NOT be used to soften any requirement in this skill or in following it.

This is a reconstruction task, not a redesign task. Copy the reference; do not reinterpret it.

## Activation

MUST run only via the `/reference-to-ui-exact` slash command. MUST NOT self-invoke from general conversation.

The first line of the first response after invocation MUST be exactly:

```
REFERENCE-TO-UI-EXACT ACTIVE
```

Arguments, if supplied, name the initial target:

`$ARGUMENTS` — expected shape: `[target route/component] [reference image path]`

If a mandatory instruction anywhere in this skill cannot be completed, MUST STOP that phase, state which instruction is blocked, explain the specific reason, and ask only for the minimum input or permission required to continue. MUST NOT silently skip, weaken, reinterpret, or substitute a mandatory instruction.

## Phase lock — MUST NOT be blended

Work MUST split into exactly two locked phases.

### Phase 1 — AUDIT ONLY

Permitted: inspecting the project, analyzing the reference image(s), measuring layout, identifying required assets, identifying functionality, asking questions, reporting feasibility limits.

MUST NOT change any code file in this phase. MUST NOT create, move, rename, or delete any file in this phase — including placeholder files, asset folders, or the manifest file.

### Phase 2 — IMPLEMENTATION

MUST NOT begin until the user has sent, verbatim, exactly:

```
APPROVE EXACT IMPLEMENTATION
```

MUST NOT interpret "go ahead", "continue", "looks good", "do your best", or any paraphrase as this approval. If the user's message is anything other than this exact string, MUST remain in Phase 1 and MUST say so explicitly.

## Project reality gate — MUST pass before any audit work

Before analyzing the design, MUST confirm all of the following:

- MUST be operating inside the user's real repository, not a scratch/temp copy.
- MUST confirm a `package.json` or equivalent manifest exists.
- MUST confirm the application code is reachable and readable.
- MUST locate the actual target screen or component in that code.
- MUST scan the assets that already exist in the project.
- MUST confirm the project can be run (dev server, build, or equivalent).
- MUST confirm the real route can be opened in a browser.
- MUST confirm the reference image is accessible at its original resolution.
- MUST confirm a tool exists for screenshotting the running page and for visual comparison.

If any item fails, MUST STOP with exactly this header:

```
PROJECT OR VERIFICATION GATE FAILED
```

...followed by which item failed and what the user must provide or fix. MUST NOT create a demo app, a standalone page, or any implementation outside the real project as a workaround. MUST NOT produce Phase 1 output until this gate passes.

## Source-of-truth order — MUST apply when evidence conflicts

1. The user's explicit instructions and answers determine behavior and scope.
2. The reference image determines layout, position, color, size, cropping, and visual hierarchy.
3. Existing project code determines the exact text and content.
4. Project architecture determines implementation method, only to the extent it does not compromise visual fidelity.

MUST NOT silently resolve a conflict between these sources. MUST STOP and ask one focused question naming the conflicting sources.

## Absolute text preservation — MUST NOT change any copy

MUST preserve all existing user-facing text exactly: letters, numbers, punctuation, sentence order, headings, labels, and meaningful whitespace.

MUST NOT rewrite, translate, shorten, expand, correct phrasing, fix spelling, or replace existing text with text read off the reference image.

Before reporting completion, MUST diff every user-facing string in every edited file against its pre-edit version and confirm zero content changes. Styling and CSS wrapping changes are allowed; string content is not.

## Asset inventory and asset folder

### Asset table — MUST produce in Phase 1, before any code changes

For every image, icon, illustration, texture, map, or photo visible in the reference, MUST add one row to this table:

| Asset | Purpose | Reference crop/location | Exact target size (px) | Format | Fit (cover/contain) | Fixed filename | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |

- "Fixed filename" MUST be decided in Phase 1, MUST NOT change afterward, and MUST be the literal filename used in code (e.g. `hero-diorama.png`).
- "Status" MUST be one of: present in project (cite path), missing.

### Folder — MUST be fixed per screen

- MUST use exactly one folder `public/reference-assets/<screen-slug>/` per screen. `screen-slug` MUST be kebab-case and MUST NOT include dates, version numbers, or suffixes like `final`, `new`, `v2`.
- MUST reference every asset in code by its final public path inside that folder (e.g. `/reference-assets/<screen-slug>/hero-diorama.png`) from the moment the code is written — even while the file does not yet exist on disk. When the user later saves a real file under that exact name into that exact folder and refreshes, it MUST appear with zero further code changes. MUST NOT use a temporary or differently-named path "for now" and rewire it later.

### `ASSET-MANIFEST.md` — MUST write exactly one, inside that folder

MUST include one entry per asset with, at minimum: filename, purpose/what it depicts, exact required dimensions in px (and @2x size if the design implies a retina asset), format (png/jpg/svg/webp), fit mode (cover/contain), and status (present/missing).

### Missing-asset behavior

A missing asset MUST NOT block building the rest of the layout. MUST build the exact-sized, exact-position, correctly cropped slot wired to the final public path instead. That empty slot MUST render as clean empty space only:

- MUST NOT show a broken-image icon.
- MUST NOT show placeholder text, lorem ipsum, a filename, or alt text visibly rendered in place of the image.
- MUST NOT show a generic placeholder icon (image icon, camera icon, etc.).
- MUST NOT fill the slot with a solid color, gradient, or pattern standing in for the real asset.
- The slot MUST reserve the correct dimensions/position so layout does not shift once the real asset is dropped in.

### No-temporary-assets rule — absolute, no exception

MUST NOT create any temporary or substitute asset under any circumstance, including but not limited to: AI-generated images, improvised or placeholder SVGs standing in for real artwork, CSS-drawn illustrations or icons standing in for a real asset, stock photos, upscaled or "enhanced" substitutes, or output from any image-generation or image-upscaling service that consumes credits. This applies even with user approval — there is no approval path that permits a temporary asset. If an asset is needed, the slot stays empty per the missing-asset behavior above until the user supplies the real file.

## Functionality mapping — MUST complete before implementation

MUST inspect existing code first for every element that appears or may be interactive: buttons, links, tabs, carousels, accordions, menus, animations, scroll effects, hover, focus, active states, forward/back navigation, progress indicators, media, keyboard navigation.

MUST NOT ask the user a question the code already answers.

MUST list every remaining unknown in this table:

| Element | Visual evidence | Unknown behavior/data | Question for user |
| --- | --- | --- | --- |

MUST consolidate every unresolved question from that table into one numbered message. MUST NOT invent functionality not evidenced by the code or the reference. MUST NOT begin implementation until all blocking answers arrive, unless the user explicitly authorized placeholders or best judgment for a named uncertainty.

## Measurement, RTL, and anti-error-hiding rules

### Measurement checklist — MUST complete before building layout

- MUST determine the reference image's true pixel dimensions and the CSS viewport width it represents; MUST NOT assume these are equal when the device-pixel ratio is unknown — MUST resolve or explicitly state the assumed scale factor.
- MUST measure, for every major region and element: position (x/y), width, height, internal padding, gaps between siblings, in px scaled to the target viewport.
- MUST sample exact colors (hex) rather than approximating with a "close enough" default.
- MUST identify font family, weight, size, and line-height for each distinct text style.
- MUST record border-radius and shadow values where visible.
- MUST perform all measurement and later verification at the same viewport width as the reference — MUST NOT measure at one width and screenshot at another.

### RTL vs. mirroring — MUST NOT be conflated

- `direction`/`dir="rtl"` governs text flow, DOM reading order, and text alignment only. It is NOT an instruction to mirror layout.
- MUST measure visual position, DOM order, `direction`, and text alignment separately.
- An element's visual side (left or right) in the reference is authoritative and MUST be reproduced on that same visual side in the implementation, regardless of the page being RTL.
- MUST NOT mirror maps, diagrams, illustrations, icons, or any region purely because the site is Hebrew/RTL.

### Prohibited error-hiding techniques

MUST NOT resolve a layout mismatch by hiding it. Specifically MUST NOT use: `overflow:hidden` to crop overflow, line-clamping, ad-hoc font shrinking, off-screen positioning, an undersized card, a hidden or removed footer/section, clipped text, or a hidden component — unless that exact behavior is clearly visible in the reference itself. A mismatch MUST be fixed at its root cause (spacing, sizing, font, container width), not masked.

## Implementation scope

- MUST reuse an existing component only if it can match the reference without breaking its shared behavior elsewhere.
- MUST create a target-local component when reuse would cause visual drift or regress another screen. MUST NOT build a new abstraction or design system for one screen.
- MUST use the project's existing styling method and conventions; MUST NOT add dependencies, replace the styling system, modify global styles, or refactor unrelated code unless required for fidelity and approved first.
- MUST limit edits to the target screen and the smallest shared primitives genuinely required.
- MUST preserve semantic HTML, accessible names, keyboard operation, and focus behavior. Accessibility work MUST NOT introduce visible deviations from the reference.
- MUST NOT modify unrelated pages/files to make the new screen feel consistent — log that idea in the recommendations file instead (see below).

## Mandatory visual verification loop

MUST NOT report completion because a build or type-check succeeded — a rendered, same-viewport, same-state screenshot comparison is required. MUST repeat the loop below until it passes or a documented limitation blocks further progress.

### The loop

1. Identify the reference's exact viewport dimensions (width × height, and DPR if known).
2. Set the browser viewport to those exact same dimensions.
3. Navigate to the real route of the real app (not a demo page).
4. Wait for fonts, images, and any animation/transition to settle before capturing.
5. Capture a screenshot of the page content only, framed the same way as the reference crop.
6. MUST NOT capture the desktop, OS chrome, browser UI, or an "Activate Windows"/OS watermark.
7. Keep the original reference image file untouched — MUST NOT resize or distort the source reference to force a match; only a working copy may be resized for comparison, and only if dimensions already differ for a documented reason.
8. Confirm the screenshot and the reference (or its working copy) share identical pixel dimensions before comparing.
9. Generate a 50%-opacity overlay of the two images.
10. Generate a pixel-difference image highlighting mismatched regions.
11. Inspect the overlay and diff for: position offsets, size mismatches, color mismatches, font/weight mismatches, spacing mismatches, missing or extra elements, clipped/cropped content, and incorrect RTL mirroring.
12. List every material mismatch found, in concrete units (px offsets, hex colors, font weights).
13. Fix each listed mismatch in code.
14. Re-screenshot and regenerate the overlay/diff after fixes.
15. Repeat steps 3–14 until the overlay/diff shows no material mismatch, or a documented limitation blocks further progress — only then proceed to the completion report.

### Failure conditions

- If screenshot/comparison tooling is unavailable, MUST STOP with exactly:

```
VISUAL VERIFICATION BLOCKED
```

- If the layout is complete but assets are still pending, status MUST be exactly:

```
LAYOUT COMPLETE — ASSETS PENDING
```

- MUST NOT claim "pixel perfect," "identical," or "one-to-one" without a same-viewport rendered comparison showing no material mismatch remains.

## Known failure modes — MUST actively prevent

These are prior, observed failures on this task. MUST NOT repeat any of them:

- Screenshotting at a resolution different from the reference.
- Replacing a topographic/textured background with a flat color.
- Replacing detailed maps with generic shapes or wave illustrations.
- Using a default/fallback font instead of the verified project font.
- Flipping the map/illustration and content between left and right because the site is RTL.
- Clipping text at the bottom of a card.
- Hiding or losing the bottom navigation/course-plan area.
- Changing page height so content falls outside the viewport.
- Delivering a screenshot that contains a Windows watermark or other OS chrome.
- Declaring the screen identical to the reference without an overlay/diff comparison.

## One cumulative consistency-recommendations file

MUST use exactly `docs/UI-CONSISTENCY-RECOMMENDATIONS.md` unless the user names another path. MUST create it if missing, otherwise update it in place. MUST NOT create a second recommendations file. MUST NOT delete prior content. MUST NOT duplicate entries — MUST update an existing entry instead.

Required section skeleton, exactly once each:

```markdown
# UI Consistency Recommendations

## Shared design language
## Reusable tokens and components
## Screen-specific findings
### [route or screen name]
## Open decisions
## Adopted decisions
```

Entries in this file are proposals only. MUST NOT implement any of them without a separate, explicit user approval for that specific change — this proposal status does not make any other instruction in this skill optional. If there is nothing new to add, MUST update the target screen's entry with the audit date and the statement `No new consistency recommendation from this reference.`

## Completion report — MUST include every item

1. What was implemented and where.
2. Assets used, or still missing (`AWAITING USER ASSET`) — never approximated, since no substitute assets are permitted.
3. Confirmed functionality and preserved existing behavior.
4. Checks performed, including viewport and visual-comparison method.
5. Remaining visual/technical differences and why they remain.
6. What was added or updated in the recommendations file.
7. The exact filenames the user must supply, and the exact folder for each.

MUST use the user's language for questions and reports. MUST keep technical identifiers and file paths unchanged.

## Stop conditions

MUST pause and ask before proceeding when:

- the target screen or authoritative content source is ambiguous;
- a missing asset materially changes the result;
- an interaction has multiple plausible behaviors requiring different code;
- exact matching would require replacing a global font, global layout, major dependency, or shared component used elsewhere;
- the reference conflicts with an explicit project requirement;
- the requested effect is unsupported, unsafe, or impossible in the target environment.

When paused, MUST provide the completed partial analysis so the user can decide quickly.
