# Dorian

> *"Every codebase has a portrait. Dorian shows you yours."*

A self-contained [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that does two things — and stops there.

1. **Reviews technical debt thoroughly** across 10 categories with severity scoring (T1 Veil → T5 Calamity).
2. **Generates one AI image-generation prompt** that anthropomorphizes the accumulated debt as a single character.

The character is the *same being across all tiers, falling*. A low-debt codebase reads as a luminous guardian angel — soft wings, faint halo, watchful posture. As debt accumulates, the figure dims, kneels, fuses with armor and decay, and finally corrupts into something eldritch. The silhouette never changes identity; the corruption changes the silhouette.

The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps shipping while a hidden portrait silently absorbs every shortcut, every aging dependency, every skipped test. Dorian makes that portrait visible.

🔗 **Live page:** https://simota.github.io/dorian/

---

## What you get from one invocation

Each invocation returns a single bundle with two artifacts.

### 1. Debt Review

Per-category scores, total score, tier, and the top findings with `path:line` evidence. Thorough enough to act on.

```yaml
review:
  total_score: 4.7
  tier: T3
  per_category:
    security:    {score: 0.82, top_findings: [...]}
    outdated_dependencies: {score: 0.71, top_findings: [...]}
    cyclomatic_complexity: {score: 0.55, top_findings: [...]}
    # ... 7 more categories
  top_findings_overall: [<top-10 across all categories>]
```

### 2. Character Prompt

A model-agnostic positive / negative prompt plus style anchors and a per-flaw inventory — every visible flaw cited back to the review. Pass it to Gemini, OpenAI, Stable Diffusion, Midjourney, or any image model. ASCII fallback when no image model is wired up.

```yaml
character_prompt:
  tier: T3
  positive_prompt: |
    A T3 Wraith — a fallen revenant, ~1.1× human height ...
    wings hardened into blackened blade-feathers; cracked iron halo ...
    Flaw surfaces:
      - toxic green aura pooling around the legs (security debt)
      - rusted shoulder pauldron and cracked greaves (outdated dependencies)
      - third arm branching from the right shoulder (cyclomatic complexity)
      ...
  negative_prompt: text, letters, modern brand, photograph, shock gore, ...
  style_anchors: [dark heroic fantasy illustration, gothic chiaroscuro, ...]
  flaws: [{category, intensity, source_finding, citation, visual_note}, ...]
```

---

## Installation

```bash
# user-level (available across all projects)
git clone https://github.com/simota/dorian.git ~/.claude/skills/dorian

# OR project-level
git clone https://github.com/simota/dorian.git .claude/skills/dorian
```

Restart Claude Code (or run `/skills`) and Dorian should appear in the skill list. Activate by mentioning the skill:

- "Dorian, summon a portrait of this repo."
- "Use Dorian to review technical debt and generate a character."
- "Run Dorian's audit-only mode against this branch."

## Modes

| Mode | When to use |
|------|-------------|
| `default` | Full bundle: debt review + character prompt |
| `audit-only` | Score only, no character — useful for CI gates |

---

## The 5 Tiers — a fall from grace

The tier comes from a formula in [`references/severity-rubric.md`](references/severity-rubric.md), never a gut feel. The same character appears at every tier, corrupting along the way.

| Tier | Band | Score | Character |
|------|------|-------|-----------|
| `T1` Veil | **Guardian** | < 1.0 | A small luminous guardian — soft wings, faint halo, watching protectively |
| `T2` Shade | **Fading** | 1.0–2.5 | A fading angel — one wing tattered, halo dimmed, kneeling under its own weight |
| `T3` Wraith | **Fallen** | 2.5–5.0 | A fallen revenant — flesh and steel fused, the wings now blackened blade-feathers |
| `T4` Revenant | **Cursed** | 5.0–8.0 | Hulking cursed body — multiple mouths breathing in unison, miasma at the feet |
| `T5` Calamity | **Eldritch** | > 8.0 | Eldritch composite — many heads, many mouths, the terrain folding into it |

### The wing motif

The wings are the strongest continuity carrier across the arc. Watch them across tiers:

| Tier | Wings |
|------|-------|
| T1 | Soft, well-kept, full plumage |
| T2 | One tattered, one intact; halo dimmed |
| T3 | Hardened into blackened blade-feathers |
| T4 | Ragged stumps, broken bone-frames |
| T5 | Coiled tendrils; the original wing is lost in the mass |

> **Note on T5.** A Calamity character is deliberately unsettling and is meant for budget conversations, not memes. Dorian asks before publishing T5 outputs to a shared dashboard.

Full visual codex (silhouette, palette, motif, environment per tier) lives in [`references/tier-codex.md`](references/tier-codex.md).

---

## What Dorian tracks

10 debt categories. Each maps 1:1 to a visible character flaw — no flaw on the canvas without evidence behind it.

| Category | Character surface |
|----------|-------------------|
| Code Smells | Body distortion, swollen joints |
| Duplication | Doppelgängers, conjoined twins |
| Cyclomatic Complexity | Extra arms, branching tendrils |
| Outdated Dependencies | Corroded armor, fused weapons |
| Test Coverage Gap | Translucent / hollow body parts |
| TODO / FIXME / HACK | Bandages, iron chains, gags |
| Architectural Violations | Tumors, conjoined growth, second head |
| Security Debt | Toxic aura, dripping sigils, parasitic worms |
| Performance Hotspots | Burning / freezing flesh, scorched limbs |
| Documentation Gap | Eyeless face, sewn mouth |

Detection methods, thresholds, and tooling hints in [`references/debt-detection.md`](references/debt-detection.md). Intensity bands and 1:1 mapping rules in [`references/trait-mapping.md`](references/trait-mapping.md).

---

## Design principles

- **Detect honestly.** No flaw on the character without `path:line` evidence behind it.
- **Score before styling.** Tier comes from a formula, never a gut feel.
- **One flaw = one debt.** Strict 1:1 mapping is what makes Dorian a diagnostic instead of decorative AI art.
- **Same being, falling.** The character is the codebase across all tiers, corrupting — not five separate creatures.
- **Sinister, not shock.** T1–T2 are celestial; T3 is the falling moment; T4–T5 are gothic horror with body distortion. Never NSFW, never shock-gore, never sexualized.

## Privacy

Dorian scrubs the image prompt before output:

- File paths longer than the module name → redacted
- Function / class names → replaced with abstract roles ("the order processor")
- Customer / company / employee names → dropped
- Internal URLs, secrets, tokens, hashes → dropped

The image prompt should read as a piece of dark fantasy / religious-painting-tradition art with no clear connection to a specific company's source code.

---

## Repository layout

```
dorian/
├── SKILL.md                     # Main skill definition
├── README.md                    # This file
├── index.html                   # GitHub Pages landing page
├── assets/styles.css            # Landing page styles
└── references/
    ├── debt-detection.md        # 10 categories, detection methods, thresholds
    ├── severity-rubric.md       # Tier scoring formula and weights
    ├── trait-mapping.md         # Findings → character flaws (1:1, intensity bands)
    ├── tier-codex.md            # Silhouette / palette / motif per tier
    └── prompt-templates.md      # AI image prompt skeleton, worked examples, ASCII fallback
```

## License

MIT (suggested — set the LICENSE file to whatever you prefer).

## Acknowledgments

The portrait metaphor is borrowed from Oscar Wilde's *The Picture of Dorian Gray* (1890), now in the public domain.
