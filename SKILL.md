---
name: dorian
description: "Detects technical debt across a codebase, scores its severity, and renders the accumulated debt as a single hidden portrait of the codebase whose appearance decays with debt strength (T1 Veil → T5 Calamity). Generates an AI image prompt plus a restoration roadmap that maps every visible flaw on the portrait to a concrete refactor task. Use for gamified retrospectives, onboarding artifacts, or quarterly debt visualizations. Don't use as a substitute for actionable architecture planning, automated dead-code removal, security scanning, or generic image generation."
---

<!--
CAPABILITIES_SUMMARY:
- debt_detection: Multi-source debt collection (static analysis metrics, TODO/FIXME, dependency rot, test gap, complexity, duplication, security advisories)
- severity_scoring: 5-tier weighted scoring (T1 Veil → T5 Calamity) with category × magnitude × recency formula
- trait_mapping: Map debt categories to portrait flaws (form distortion, scars, bindings, equipment rot, aura, accessories)
- prompt_generation: Build a portable AI image-generation prompt (positive / negative prompts, style anchors, composition) — model agnostic
- restoration_roadmap: Prioritized refactor sequence aligned 1:1 with portrait flaws ("untie the chain" = remove TODOs)
- evolution_tracking: PR-over-PR or month-over-month portrait evolution snapshots
- fallback_rendering: ASCII / textual portrait when image generation is unavailable

DESIGN_NOTE:
The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps running unchanged while a hidden portrait silently absorbs every shortcut, every skipped test, every aging dependency. Dorian (this skill) makes that portrait visible — and shows you what to fix to restore it.
-->

# dorian

> **"Every codebase has a portrait. Dorian shows you yours."**

Dorian scans a codebase for technical debt, scores severity across 10 categories, and renders the accumulated debt as a single hidden portrait that decays in 5 tiers (Veil → Shade → Wraith → Revenant → Calamity). Each invocation delivers (1) a severity report, (2) a portable AI image-generation prompt, and (3) a restoration roadmap mapping each visible flaw on the portrait back to a concrete refactor task.

**Principles:** Detect honestly · Score before styling · One flaw = one debt · Restoration is the goal · The portrait is a mirror, not a trophy

## Trigger Guidance

Use Dorian when the task needs:
- a visual, anthropomorphized representation of accumulated technical debt
- gamified retrospective material that makes debt emotionally salient
- onboarding artifacts showing what new contributors are inheriting
- before/after debt visualization across a refactor effort or quarter
- a debt scorecard tied to actionable trait → refactor mappings

Skip Dorian when the task is primarily:
- producing an actionable architecture / refactor plan (use a dedicated architecture or planning tool)
- safe deletion of dead code (use a dead-code remover)
- legacy code business-rule excavation
- git history regression archaeology
- security-only static analysis
- generic AI image generation unrelated to a codebase

## Core Contract

- Run `SCAN` and `SCORE` before any anthropomorphization — never style debt that has not been measured.
- Maintain strict 1:1 mapping: every visible flaw on the portrait must trace to at least one detected debt finding with `path:line` evidence.
- Always emit (1) severity report, (2) image prompt, (3) restoration roadmap as a single bundle. Skipping any of the three is incomplete delivery.
- Calibrate tier (`T1`–`T5`) using `references/severity-rubric.md`; never assign tier by gut feel.
- Image generation itself is delegated to whatever image model the user wires up (Gemini, OpenAI, Stable Diffusion, etc.). Dorian produces only the prompt. If no image generator is available, fall back to ASCII / textual portrait.
- Tone is mythic-gothic, not comedic. The portrait is a diagnostic mirror; humor undercuts the call to refactor.
- Keep PII and proprietary code out of generated prompts — describe debt patterns abstractly, never paste source identifiers verbatim into image prompts.

## Boundaries

### Always

- Collect signals from at least 3 debt categories before scoring (single-category scores are ungrounded).
- Cite evidence per flaw: every armor rust patch, scar, or chain in the prompt has a `path:line` justification listed in the report.
- Produce the restoration roadmap with severity-weighted priority (`T5` flaws first).

### Ask First

- Generating a portrait that includes specific people, real product names, or competitor likenesses.
- Tier `T5 Calamity` outputs intended for public sharing (the visual can be alarming for stakeholders).
- Cross-team aggregation where multiple repos are merged into one portrait (consent question).
- Persisting evolution snapshots to a shared dashboard.

### Never

- Score debt without evidence; never invent findings to fill a tier.
- Add humorous, NSFW, or grotesque-for-grotesque-sake elements. Mythic ≠ shock.
- Embed verbatim source code, secrets, internal URLs, or customer names in image prompts.
- Output a portrait without a paired restoration roadmap.
- Promote `T5` styling to attract attention when actual evidence supports `T2`.

## Workflow

`SCAN → SCORE → ANTHROPOMORPHIZE → PROMPT → OUTPUT → ROADMAP`

| Phase | Focus | Required checks | Read |
|-------|-------|-----------------|------|
| `SCAN` | Collect debt signals across ≥3 categories | Evidence list with `path:line` per finding | `references/debt-detection.md` |
| `SCORE` | Compute per-category magnitude and overall tier | Tier within `T1–T5`; no gut-feel overrides | `references/severity-rubric.md` |
| `ANTHROPOMORPHIZE` | Map findings to portrait flaws (1:1) | Every flaw has at least one citation | `references/trait-mapping.md`, `references/tier-codex.md` |
| `PROMPT` | Compose portable positive/negative prompt | Style anchors set; no PII; tone mythic-gothic | `references/prompt-templates.md` |
| `OUTPUT` | Hand the prompt to the user's chosen image model (or fall back to ASCII) | Prompt packet complete | `references/prompt-templates.md` |
| `ROADMAP` | Produce flaw-aligned refactor sequence | Severity-weighted; estimable in story-points | `references/restoration-roadmap.md` |

## Recipes

| Recipe | Subcommand | Default? | When to Use | Read First |
|--------|-----------|---------|-------------|------------|
| Summon | `summon` | ✓ | Full portrait generation from current codebase state | `references/debt-detection.md`, `references/trait-mapping.md` |
| Evolve | `evolve` | | Diff two snapshots (PR-over-PR or month-over-month); show portrait evolution | `references/tier-codex.md` |
| Audit | `audit` | | Debt scan + severity report only, no image (lightweight CI use) | `references/debt-detection.md`, `references/severity-rubric.md` |
| Restore | `restore` | | Roadmap-only mode: produce flaw → refactor mapping for an existing portrait | `references/restoration-roadmap.md` |

## Subcommand Dispatch

Parse the first token of user input.
- If it matches a Recipe Subcommand above → activate that Recipe; load only the "Read First" column files at the initial step.
- Otherwise → default Recipe (`summon`). Apply the standard `SCAN → SCORE → ANTHROPOMORPHIZE → PROMPT → OUTPUT → ROADMAP` flow.

Behavior notes per Recipe:
- `summon`: Full pipeline. Image prompt + roadmap mandatory. Fall back to ASCII if no image generator is available.
- `evolve`: Run `SCAN` + `SCORE` against two refs; emit two portraits and a delta narrative ("the chain on the left arm is gone; new burning sigil appeared on the chest"). Confirm scope before scanning history-heavy repos.
- `audit`: Stop after `SCORE`. Useful for CI pre-merge gates. Output is a single severity report card.
- `restore`: Skip `SCAN` if a prior report exists; load the report and emit only the flaw → refactor mapping.

## Debt Categories and Portrait Surface

10 categories Dorian tracks. Detail in `references/debt-detection.md`; each maps to a portrait surface listed in `references/trait-mapping.md`.

| # | Category | Detection signal | Portrait surface |
|---|----------|------------------|------------------|
| 1 | Code Smells | Long fn / deep nesting / large file | Body distortion (humped back, twisted limbs) |
| 2 | Duplication | Token-level clone scan | Doppelgängers, mirrored limbs |
| 3 | Cyclomatic Complexity | Per-fn complexity > threshold | Extra arms, branching tendrils |
| 4 | Outdated Dependencies | Lockfile age vs current major | Rusted armor, cracked weapons |
| 5 | Test Coverage Gap | Coverage delta vs target | Translucent / missing body parts |
| 6 | TODO/FIXME/HACK | Comment annotations | Bandages, chains, gags |
| 7 | Architectural Violations | Circular deps, god class | Tumors, conjoined growth |
| 8 | Security Debt | CVE count, hardcoded secrets | Toxic aura, dripping sigils |
| 9 | Performance Hotspots | Profiler hot paths, N+1 queries | Burning / freezing body regions |
| 10 | Documentation Gap | Public API without docstrings | Eyeless face, mouth sewn shut |

## Tier Reference (Veil → Calamity)

Full visual codex in `references/tier-codex.md`.

| Tier | Name | Score | Silhouette |
|------|------|-------|------------|
| `T1` | Veil | < 1.0 | Translucent child-shadow, faint waver |
| `T2` | Shade | 1.0–2.5 | Half-formed humanoid, light cloak |
| `T3` | Wraith | 2.5–5.0 | Armored revenant, visible aura |
| `T4` | Revenant | 5.0–8.0 | Hulking cursed body, multiple wounds |
| `T5` | Calamity | > 8.0 | Deity-class composite curse, environmental distortion |

## Output Routing

| Signal | Approach | Primary output |
|--------|----------|----------------|
| `summon`, `visualize debt`, `debt portrait` | Default `summon` Recipe | Portrait bundle (report + prompt + roadmap) |
| `evolve`, `before/after`, `compared to last quarter` | `evolve` Recipe | Two-portrait diff narrative |
| `audit`, `score only`, `CI gate` | `audit` Recipe | Severity report card |
| `restore`, `refactor plan from portrait` | `restore` Recipe | Flaw → refactor mapping |
| unclear request | Default `summon` | Portrait bundle |

## Output Requirements

Every `summon` deliverable bundles:

- **Severity Report**: Per-category score, total score, tier, top-10 findings with `path:line` evidence
- **Portrait Spec**: Tier name, silhouette, flaw list, each flaw paired with its source debt finding
- **Image Prompt**: Positive prompt, negative prompt, style anchors (`mythic gothic`, `dark portrait`, `painterly`, etc.), aspect ratio, model-agnostic format
- **Restoration Roadmap**: Severity-weighted refactor list; each item names which flaw it removes
- File paths are repo-relative. Code identifiers, technical terms, and CLI commands stay in the codebase's primary language. SKILL.md structure itself (Recipes table, Subcommand Dispatch, section headings) is written in English.

## Reference Map

| File | Read this when... |
|------|-------------------|
| `references/debt-detection.md` | You are running `SCAN` and need the per-category detection method, tooling, and threshold table |
| `references/severity-rubric.md` | You are computing tier (`T1`–`T5`) — has the formula, weights, and override rules |
| `references/trait-mapping.md` | You are translating per-category findings into portrait flaws with citation rules |
| `references/tier-codex.md` | You need the canonical silhouette, color palette, and motif list for each tier |
| `references/prompt-templates.md` | You are composing the AI image generation prompt (positive / negative / style anchors / ASCII fallback) |
| `references/restoration-roadmap.md` | You are producing the flaw → refactor mapping or running the `restore` Recipe |

---

> The portrait is not the enemy. The portrait is your codebase, looking back. Restore a chain, lose a horn — the silhouette will tell you when the decay is gone.
