# Restoration Roadmap

How Dorian turns the portrait spec into an actionable refactor roadmap. Used during the `ROADMAP` phase. Without the roadmap, Dorian is just AI art — the roadmap is what makes it diagnostic.

## Core Principle: Flaw → Refactor (1:1)

For every visible flaw on the portrait, the roadmap names exactly one refactor task that, when shipped, will remove that flaw from the next snapshot. This is what gives users a feedback loop: "we untied the chain on the right arm" should be a literal sentence after the next run.

## Roadmap Item Schema

```yaml
- id: R-001
  removes_flaw: "rusted shoulder pauldron"
  category: outdated_dependencies
  intensity_addressed: 0.7
  task: "Upgrade react 16 → 19 in the checkout package"
  evidence_anchor: "package-lock.json:1024"
  effort: M           # XS | S | M | L | XL  (≈ <2h | <1d | 1-3d | 1w | >1w)
  risk: medium        # low | medium | high   (likelihood of regression)
  prerequisites: []
  expected_visual_change: "shoulder pauldron returns to clean steel"
```

## Prioritization

Roadmap items are sorted by:

```
priority = intensity_addressed × category_weight × (1 / effort_cost)
                              − risk_penalty
```

Where:

- `category_weight` reuses the weights from `severity-rubric.md`
- `effort_cost`: XS=1, S=2, M=4, L=8, XL=16
- `risk_penalty`: low=0, medium=0.2, high=0.5

Top of the list = "biggest visual change for the cheapest, safest fix."

## Phasing

Group roadmap items into 3 phases. The user (or their planning tool) executes them in order:

| Phase | Theme | Typical content |
|-------|-------|-----------------|
| `Phase 1 — Banish` | Quick wins, top severity | Critical CVE patches, top TODOs older than 180 days, easiest dep bumps |
| `Phase 2 — Bind` | Structural fixes | Test coverage backfill on the dominant module, breaking up god classes, reducing duplication |
| `Phase 3 — Restore` | Long-term form | Architectural refactor, full dep modernization, documentation rewrite |

Each phase ends with a re-run of Dorian (default `summon`) to verify the silhouette changed. Visualizing progress is the point.

## Output Format

```markdown
## Restoration Roadmap — Tier T3 Wraith (Score 4.7)

### Phase 1 — Banish (target: drop to T2 in ~2 weeks)

1. **Patch CVE-2024-XXXX in `lib/auth`** — removes the toxic green aura
   - Effort: S | Risk: low
   - Evidence: `package-lock.json:1024`
   - Expected: aura clears

2. **Untie 8 TODOs older than 180 days in `src/checkout`** — removes parchment seals
   - Effort: M | Risk: low
   - Evidence: `src/checkout/process.ts:142, 198, 240, …`
   - Expected: forearm bindings fall away

…

### Phase 2 — Bind (target: drop to T2 confirmed)

…

### Phase 3 — Restore (target: T1 by next quarter)

…

### Re-run cadence

- After each phase, run `dorian summon` to update the portrait.
- After phase 3, run `dorian evolve` against the original snapshot for the diptych.
```

## Effort Calibration

Dorian does not estimate effort from thin air — calibrate against:

- The size of the affected module (LOC, file count)
- Historical effort estimates if your team tracks them
- The blast radius of the affected code (how many call sites touch it)
- If no calibration is available, mark the effort as `M ?` (best-guess medium with explicit uncertainty)

## Risk Assessment

| Risk Level | Trigger |
|------------|---------|
| `low` | Pure removal (TODOs, dead code, lint fixes); deps with no breaking changes |
| `medium` | Refactor of business logic; minor framework upgrade; dep with deprecations |
| `high` | Major framework upgrade; god-class breakup; data model change; architectural shift |

If a single roadmap item is `high` risk, attach a "see architectural review" note — Dorian stops short of architectural decision authority.

## What the Roadmap Is Not

- It is **not** an ADR. Dorian names *what* would remove the flaw; an architecture document names *how* and *why*.
- It is **not** a sprint plan. The phasing is logical-order, not capacity-aware. A sprint planner consumes this list, doesn't replace it.
- It is **not** a guarantee. A team could ship every Phase-1 item and still see T3 if new debt accumulates faster than it's restored. Dorian measures, doesn't prescribe how to allocate engineering time.

## Empty Roadmap (T1 Veil)

If the codebase is `T1`, the roadmap is intentionally short:

```markdown
## Restoration Roadmap — Tier T1 Veil (Score 0.6)

The codebase carries minimal debt. The portrait is faint.

Recommended cadence:
- Re-run `dorian audit` monthly to catch drift early.
- No active refactor required. The portrait is a healthy snapshot, not a call to action.
```

Resist the urge to invent work. T1 means the portrait is doing its job by being unalarming.
