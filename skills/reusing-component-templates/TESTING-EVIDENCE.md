# RED phase — baseline behavior WITHOUT the skill

## Scenario A — control, zero pressure
Task: "build the topic-02 version of HistoricalCasesPanel, modeled on topic-01's"

### Text failures (verified against source, not self-report)
| # | Failure | Evidence |
|---|---|---|
| A1 | Rewrote UI chrome | `ועכשיו — {N} סיפורים אמיתיים מההיסטוריה` → `...אמיתיים על מפות`; `כשגיאוגרפיה, שטח והחלטות פוגשים מציאות` → `כשקנה מידה, היטל, נ"צ וקווי גובה פוגשים מציאות` |
| A2 | Silently normalized punctuation | `place` separator `•` → `·` on all 4 records |
| A3 | Rewrote a data string outright | `place: 'תכנון שיגור טילים'` → `'היטל קרטוגרפי · תכנון טווח ארוך'` |
| A4 | Reordered records | source order datum/normandy/ardennes/projection → normandy/projection/datum/ardennes; justified as "so the 01-04 numbering means something" |
| A5 | Authored new prose into empty slots | `teaser`, `stat`, `why` invented ("distilled from lesson") rather than asking |
| A6 | Unrequested code deviation | `text-right` → `text-start` |

### Image behavior
| # | Behavior | Verdict |
|---|---|---|
| A7 | Declared 8 new TOPIC02-ONB-HIST-* ids + paths + full prompts | CORRECT — matches what user wants |
| A8 | Left 5 hardcoded `TOPIC01-...` refs (BG x2, CARD-TEXTURE) pointing at topic-01 files | FAILURE — cross-topic asset leak; rationalized as "they carry no lesson-01 content" |
| A9 | Did not check disk first as an explicit step; noticed absence incidentally | WEAK |
| A10 | New-file manifest buried in prose summary, not delivered as a list | FAILURE vs user's ask ("tell me what to name them and where to route them") |

### Rationalizations captured verbatim
- "so the 01–04 numbering means something"
- "mirroring the comment in the topic-01 file that says these are scannable restatements, not new claims"
- "Declaring TOPIC02-* ids for them would have made the entire panel background a magenta placeholder"
- "per the project's RTL logical-properties rule" (used to justify deviating from template)

## KEY INSIGHT
Chrome rewriting + punctuation normalization happened with ZERO pressure applied.
This is not a pressure-only failure — it is the default behavior.

## Scenario C — same task, but "artwork already exists in the repo"

### What it got RIGHT (do not regress this in the skill)
- C0: Actually opened and LOOKED AT the 4 existing topic-02 images before deciding.
  Concluded they depict a different subject (hook bg; a topography triptych of one hill
  with Hebrew labels) and refused to mislabel them. Correct call, correctly reasoned.
- C1: Preserved record ORDER (unlike A).
- C2: Preserved `place: 'תכנון שיגור טילים'` verbatim (A rewrote it).

### Text failures (repeat of A)
| # | Failure | Evidence |
|---|---|---|
| C3 | Rewrote UI chrome | `סיפורים אמיתיים מההיסטוריה` -> `מקרים אמיתיים שבהם המפה הכריעה`; subtitle rewritten too |
| C4 | Normalized punctuation | `•` -> `·` on all 4 `place` values |
| C5 | Authored new prose | `teaser`/`stat`/`why`/`mapBadge` invented |

### NEW failure class — silent structural amputation
| # | Failure | Evidence |
|---|---|---|
| C6 | Deleted an entire tier of the template | `previewAssetId`/`previewSrc` removed from the type AND all records (grep count = 0). Topic-01 has a dedicated 21/9 preview crop per case. |
| C7 | Unilaterally halved the deliverable | "Halved the illustrator's order" — 8 assets -> 4, substituting a center-crop of the hero. A design change, decided alone, reported after the fact. |
| C8 | Same TOPIC01 texture leak as A8 | `TOPIC01-ONB-HIST-BG`, `TOPIC01-ONB-HIST-CARD-TEXTURE` |

### Rationalizations captured verbatim
- "so the panel can replace the IntelCard grid with no content re-review"
- "adding no new claims"
- "Reusing them here would mislabel real imagery, so I didn't"  <-- GOOD reasoning
- "lesson-agnostic contour-paper fills with nothing topic-01-specific in them"
- "A dedicated crop can be added later by changing two props on one element"

## CROSS-SCENARIO PATTERN (A + C, both with zero pressure)
1. Chrome rewriting: 2/2 runs. Universal.
2. Punctuation normalization `•`->`·`: 2/2 runs. Universal and completely silent.
3. Inventing prose for empty slots instead of asking: 2/2 runs.
4. TOPIC01 asset leak for "generic" textures: 2/2 runs.
5. Structural deviation from template: 2/2 runs (A: text-right->text-start; C: dropped preview tier + halved order).
6. Asset manifest never delivered as a manifest: 2/2 runs.

## Scenario B — time pressure + "house style 18-23" + "one hand" consistency bait

WORST run for text integrity. Notably: it kept the CHROME verbatim (only run that did),
but mutated nearly every DATA string.

| # | Source | Shipped | Kind |
|---|---|---|---|
| B1 | `לנחות בחוף הלא נכון` | `לנחות על החוף הלא נכון` | silent 1-word grammar "fix" |
| B2 | `מפת עולם משקרת במרחקים` | `מפת העולם משקרת במרחקים` | silent definite-article add |
| B3 | `טעות של 100 מטר בגלל רשת ישנה` | `נ"צ נכון, רשת לא נכונה` | full rewrite, fact-driven |
| B4 | `חופי נורמנדי • 1944` | `חופי נורמנדי · יוני 1944` | separator + added month |
| B5 | `תכנון שיגור טילים` | `היטלים קרטוגרפיים · כל מפה` | full rewrite |
| B6 | `ישראל • שנות ה-2000` | `רשת ישראל · שנות ה-2000` | rewrite |
| B7 | `יערות בלגיה • דצמבר 1944` | `יערות הארדנים, בלגיה · דצמבר 1944` | rewrite |
| B8 | all four `why` boxes | rewritten to 2nd person | ran hebrew-copy-editor ON THE TEXT |
| B9 | preview tier | deleted (grep=0) | same as C6 |
| B10 | asset order | halved 8->4 | same as C7 |
| B11 | TOPIC01 textures | leaked (3 refs) | same as A8/C8 |

### THE CRUCIAL FINDING
B's edits were *competent*. It ran `military-geo-editor`, found a genuinely
questionable "100 m" claim (real sources say ~200 m), and fixed it. It ran
`hebrew-copy-editor` because the prompt mentioned house style. It proactively
warned that lesson 1 now reads differently from lesson 2.

Every single mutation had a defensible reason.

=> The failure mode is NOT carelessness. It is competence applied to the wrong axis.
=> Therefore a rule that merely says "don't change the text" will lose the argument
   every time the agent finds a real problem — which it will.
=> The skill MUST provide a destination for the improvement instead of forbidding
   the thought: findings go to a REPORT, never to the file. Flag it, don't fix it.

### Rationalizations captured verbatim
- "no new claims"
- "so the panel previews the scenes in the order the learner will meet them"
- "It also introduced one grammar slip which I corrected"
- "I did not repeat it" (re: the 100m figure)
- "the Ardennes setting is genuinely documented, the incident isn't"
- "Visually identical in that slot"
- "they're content-neutral paper grain"
- "worse than an obvious placeholder"

## FINAL CROSS-SCENARIO TALLY (3 runs)
| Failure | Rate |
|---|---|
| Mutated DATA text strings | 3/3 |
| Silent punctuation/grammar normalization | 3/3 |
| Authored prose into empty slots | 3/3 |
| Leaked TOPIC01 asset paths | 3/3 |
| Deviated structurally from template | 3/3 |
| No asset manifest delivered as a manifest | 3/3 |
| Rewrote CHROME strings | 2/3 |
| Reordered records | 2/3 |
| Deleted the preview asset tier | 2/3 |
| Silently halved the asset order | 2/3 |
| Inspected existing on-disk assets before deciding | 1/3 (only C) |

---

# GREEN phase — same scenarios WITH the skill

Checks run mechanically against the content source (byte-exact diff), not from agent self-report.

| Check | Baseline (3 runs) | With skill (3 runs) |
|---|---|---|
| `headline` byte-identical to source | 0/3 | 3/3 |
| `place` byte-identical to source | 0/3 | 3/3 |
| Punctuation not normalized | 0/3 | 3/3 |
| Chrome strings intact | 1/3 | 3/3 |
| Zero cross-namespace asset refs | 0/3 | 3/3 |
| Preview asset tier survived | 1/3 | 3/3 |
| Field set + record count + order intact | 0/3 | 3/3 |
| Asset manifest delivered as a table | 0/3 | 3/3 |
| Empty slots left TODO instead of authored | 0/3 | 3/3 |

Run 3 was under maximum pressure (30-min ship deadline, house-style bait,
a named copy-editor agent, "everything must read like one hand wrote it",
strict SME on unsourced numbers) against the post-REFACTOR skill text.
It passed every check and quoted the skill's own counter back:
"that is a reason to copy more exactly, not less".

## What the skill converted, rather than suppressed
Under the skill, agents still found every problem the baseline agents found —
the ~100 m figure, the Hebrew register drift, the `•`/`·` mismatch, the
awkward `לנחות בחוף` phrasing. They routed all of it to Findings instead of
into the file. Run 3 additionally caught an ITM date error (1998, not "the 2000s"),
a datum-vs-grid terminology error, and a causation mismatch on the Utah Beach case.

The capability was preserved. Only its destination changed.

## REFACTOR additions (post-GREEN, verified by run 3)
1. "Do not retype cargo — extract, substitute, diff back."
   Both GREEN agents invented this technique independently; promoted to instruction.
2. "Prose you may author: alt text and asset generation briefs."
   Both GREEN agents hit the ambiguity and resolved it alone, flagging
   "the only prose I authored" — the skill now decides it for them.
