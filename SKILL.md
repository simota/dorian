---
name: dorian
description: "Reviews a codebase for technical debt across 10 categories with severity scoring (T1 Veil → T5 Calamity), then generates a single AI image-generation prompt that anthropomorphizes the accumulated debt as one sinister character. Two outputs per invocation: (1) a thorough debt review with path:line evidence, (2) a model-agnostic character prompt. Use for gamified retrospectives, onboarding artifacts, or quarterly debt visualizations. Don't use as a substitute for actionable architecture planning, automated dead-code removal, security scanning, or generic image generation."
---

<!--
CAPABILITIES_SUMMARY:
- debt_review: Multi-source debt collection (static analysis, TODO/FIXME, dependency rot, test gap, complexity, duplication, security advisories) with per-category severity scoring
- severity_scoring: 5-tier weighted scoring (T1 Veil → T5 Calamity) using a category × magnitude × recency formula
- character_prompt: Build a portable AI image-generation prompt (positive / negative prompts, style anchors, composition) — model agnostic, gothic-horror tone
- trait_mapping: Map debt categories to character surfaces (body distortion, weeping wounds, bindings, corroded equipment, toxic aura)
- fallback_rendering: ASCII / textual character when image generation is unavailable

DESIGN_NOTE:
The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps running unchanged while a hidden portrait silently absorbs every shortcut, every skipped test, every aging dependency. Dorian (this skill) makes that portrait visible — without prescribing a fix path. The review is the diagnosis; the character is the diagnosis made visible.
-->

# dorian

> **"Every codebase has a portrait. Dorian shows you yours."**

Dorian does two things, in order:

1. **Reviews technical debt thoroughly.** Scans 10 categories, computes per-category and total severity, assigns a tier (T1 Veil → T5 Calamity), and lists the top findings with `path:line` evidence.
2. **Generates a character prompt.** Translates the review into a portable AI image-generation prompt that anthropomorphizes the debt as a single sinister character. Pass it to any image model (Gemini, OpenAI, Stable Diffusion, Midjourney). ASCII fallback when no image model is wired up.

The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps shipping while a hidden portrait silently absorbs every shortcut, every aging dependency, every skipped test. Dorian makes that portrait visible.

**Principles:** Detect honestly · Score before styling · One flaw = one debt · The portrait is a mirror — and the mirror has teeth

## Trigger Guidance

Use Dorian when the task needs:
- a thorough technical-debt review with severity scoring
- a single-character visualization of that debt for retrospectives, onboarding, or quarterly debt reports
- a debt scorecard paired with one piece of evidence-grounded character art
- before/after debt comparison across two refs (run twice, compare reviews and characters side by side)

Skip Dorian when the task is primarily:
- producing an actionable refactor / migration plan (Dorian gives diagnosis and evidence; sequencing is up to the team)
- safe deletion of dead code
- legacy code business-rule excavation
- git history regression archaeology
- security-only static analysis
- generic AI image generation unrelated to a codebase

## Core Contract

- Run `SCAN` and `SCORE` before generating the character — never anthropomorphize debt that has not been measured.
- Maintain strict 1:1 mapping: every visible flaw on the character must trace to at least one detected debt finding with `path:line` evidence.
- Always emit both outputs as a single bundle: (1) the debt review and (2) the character prompt. Skipping either is incomplete delivery.
- Calibrate tier (`T1`–`T5`) using `references/severity-rubric.md`; never assign tier by gut feel.
- Image generation itself is delegated to whatever image model the user wires up. Dorian produces only the prompt. If no generator is available, fall back to ASCII / textual character.
- Tone follows a fall-from-grace arc keyed to tier. T1–T2 are celestial: a guardian angel, dimming. T3 is the moment of falling: divine forms fused with armor and decay. T4–T5 are gothic horror: body distortion, weeping wounds, oppressive miasma, parasitic growths, eldritch dread. Never NSFW, never shock-gore-for-shock, never sexualized.
- Keep PII and proprietary code out of generated prompts — describe debt patterns abstractly, never paste source identifiers verbatim.

## Boundaries

### Always

- Collect signals from at least 3 debt categories before scoring (single-category scores are ungrounded).
- Cite evidence per flaw: every chain, every rusted plate, every toxic sigil in the prompt has a `path:line` justification recorded in the review.
- Run a PII scrub pass over the final prompt before delivering it.

### Ask First

- Generating a character that includes specific people, real product names, or competitor likenesses.
- Tier `T5 Calamity` outputs intended for public sharing — the imagery is deliberately unsettling.
- Cross-team aggregation where multiple repos are merged into one character (consent question).

### Never

- Score debt without evidence; never invent findings to fill a tier.
- Add humorous, NSFW, sexualized, or shock-gore elements. Sinister ≠ degrading.
- Embed verbatim source code, secrets, internal URLs, or customer names in the prompt.
- Output a character without a paired debt review.
- Promote `T5` styling to attract attention when actual evidence supports `T2`.

## Workflow

`SCAN → SCORE → CHARACTERIZE → PROMPT`

| Phase | Focus | Required checks | Read |
|-------|-------|-----------------|------|
| `SCAN` | Collect debt signals across ≥3 categories | Evidence list with `path:line` per finding | `references/debt-detection.md` |
| `SCORE` | Compute per-category magnitude and overall tier | Tier within `T1–T5`; no gut-feel overrides | `references/severity-rubric.md` |
| `CHARACTERIZE` | Map findings to character flaws (strict 1:1) | Every flaw has at least one citation | `references/trait-mapping.md`, `references/tier-codex.md` |
| `PROMPT` | Compose model-agnostic positive/negative prompt | Style anchors set; PII scrubbed; tone gothic horror | `references/prompt-templates.md` |

## Modes

Two modes. Default delivers both outputs; the alternative is for lightweight CI use.

| Mode | When | Output |
|------|------|--------|
| default | "summon", "review debt with a portrait", "show this codebase as a character", or any unclear request | Debt Review + Character Prompt |
| `audit-only` | CI gate, score-only request, or explicit "no image" hint | Debt Review only (skip `CHARACTERIZE` + `PROMPT`) |

## Debt Categories and Character Surface

10 categories Dorian scans. Detail in `references/debt-detection.md`; each maps to a character surface listed in `references/trait-mapping.md`.

| # | Category | Detection signal | Character surface |
|---|----------|------------------|-------------------|
| 1 | Code Smells | Long fn / deep nesting / large file | Body distortion, swollen joints |
| 2 | Duplication | Token-level clone scan | Doppelgängers, conjoined twins |
| 3 | Cyclomatic Complexity | Per-fn complexity > threshold | Extra arms, branching tendrils |
| 4 | Outdated Dependencies | Lockfile age vs current major | Corroded armor, fused weapons |
| 5 | Test Coverage Gap | Coverage delta vs target | Translucent / hollow body parts |
| 6 | TODO/FIXME/HACK | Comment annotations | Bandages, iron chains, gags |
| 7 | Architectural Violations | Circular deps, god class | Tumors, conjoined growth, second head |
| 8 | Security Debt | CVE count, hardcoded secrets | Toxic aura, dripping sigils, parasitic worms |
| 9 | Performance Hotspots | Profiler hot paths, N+1 queries | Burning / freezing flesh, scorched limbs |
| 10 | Documentation Gap | Public API without docstrings | Eyeless face, sewn mouth |

## Tier Reference (Veil → Calamity)

Full visual codex in `references/tier-codex.md`. The arc is a fall from grace: a low-debt codebase reads as an angelic guardian; as debt accumulates the figure dims, falls, and corrupts into something monstrous.

| Tier | Name | Score | Silhouette |
|------|------|-------|------------|
| `T1` | Veil | < 1.0 | A small luminous guardian — soft wings, a faint halo, watching protectively |
| `T2` | Shade | 1.0–2.5 | A fading angel — one wing tattered, halo dimmed, kneeling under its own weight |
| `T3` | Wraith | 2.5–5.0 | A fallen revenant — flesh and steel fused, the wings now blackened blades |
| `T4` | Revenant | 5.0–8.0 | A hulking cursed body — multiple mouths breathing in unison, miasma at the feet |
| `T5` | Calamity | > 8.0 | An eldritch composite — many heads, many mouths, the terrain folding into it |

## Output Requirements

Every default invocation delivers exactly two artifacts.

### 1. Debt Review

```yaml
review:
  total_score: <float>            # weighted sum across categories
  tier: T1 | T2 | T3 | T4 | T5
  per_category:
    code_smells:            {score, top_findings: [{path, line, note}]}
    duplication:            {score, top_findings: [...]}
    cyclomatic_complexity:  {score, top_findings: [...]}
    outdated_dependencies:  {score, top_findings: [...]}
    test_coverage_gap:      {score, top_findings: [...]}
    todo_fixme_hack:        {score, top_findings: [...]}
    architectural:          {score, top_findings: [...]}
    security:               {score, top_findings: [...]}
    performance:            {score, top_findings: [...]}
    documentation:          {score, top_findings: [...]}
  top_findings_overall: [<top-10 across all categories>]
  notes: <salient context, repo skew, false-positive caveats>
```

### 2. Character Prompt

```yaml
character_prompt:
  tier: T3
  total_score: 4.7
  positive_prompt: <string>       # tier silhouette + flaw inventory + palette + style anchors
  negative_prompt: <string>
  style_anchors: [gothic horror dark portrait, painterly, dramatic chiaroscuro, ...]
  aspect_ratio: "2:3"
  resolution: "1024x1536"
  flaws:                          # one entry per visible flaw, all evidenced
    - category: outdated_dependencies
      intensity: 0.7
      source_finding: F-014
      citation: "package-lock.json:1024 (react@16, current major @19)"
      visual_note: "rust concentrated on chest plate"
    - ...
  pii_scrub_result: passed
  notes: <e.g. "T5 — confirm with user before publishing">
```

File paths are repo-relative. Code identifiers, technical terms, and CLI commands stay in the codebase's primary language. SKILL.md structure itself (table headings, phase names) is written in English.

## Reference Map

| File | Read this when... |
|------|-------------------|
| `references/debt-detection.md` | Running `SCAN` — per-category detection method, tooling, threshold table |
| `references/severity-rubric.md` | Computing tier (`T1`–`T5`) — formula, weights, override rules |
| `references/trait-mapping.md` | Translating findings into character flaws with citation rules |
| `references/tier-codex.md` | Canonical silhouette, palette, and motif inventory per tier |
| `references/prompt-templates.md` | Composing the AI image prompt (positive / negative / style anchors / ASCII fallback) |

---

> The character is not a costume the codebase wears. The character is what the codebase has been quietly becoming. Dorian only paints what is already there.
