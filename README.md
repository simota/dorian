# Dorian

> *"Every codebase has a portrait. Dorian shows you yours."*

Dorian is a self-contained [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that scans a codebase for technical debt and renders the accumulated debt as a single hidden portrait — a character whose appearance decays in 5 tiers (Veil → Shade → Wraith → Revenant → Calamity) as your debt grows.

The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps running unchanged while a hidden portrait silently absorbs every shortcut, every skipped test, every aging dependency. Dorian makes that portrait visible — and tells you what to fix to restore it.

## What you get from one invocation

Each `summon` returns a single bundle:

1. **Severity Report** — per-category scores, total score, tier (T1–T5), top findings with `path:line` evidence.
2. **Portrait Spec** — tier silhouette, list of visible flaws, each flaw cited back to a specific debt finding.
3. **AI Image Prompt** — model-agnostic positive / negative prompts and style anchors. Pass to Gemini, OpenAI, Stable Diffusion, or any image model. ASCII fallback when no image model is available.
4. **Restoration Roadmap** — phased refactor list (Banish → Bind → Restore). Every item names which flaw on the portrait it removes, so the next snapshot visibly improves.

## Installation

Dorian is a single Claude Code skill folder. Drop it into your skills directory:

```bash
# user-level (available across all projects)
git clone https://github.com/<you>/dorian.git ~/.claude/skills/dorian

# OR project-level
git clone https://github.com/<you>/dorian.git .claude/skills/dorian
```

Restart Claude Code (or run `/skills`) and Dorian should appear in the skill list. Activate by mentioning the skill in any of these phrasings:

- "Dorian, summon a portrait of this repo."
- "Run `dorian audit` against the current branch."
- "Use Dorian to compare main vs this PR."

## Recipes

| Recipe | When to use |
|--------|-------------|
| `summon` (default) | Full portrait + report + roadmap |
| `evolve` | Diff two snapshots; show how the portrait changed |
| `audit` | Score only — no image — useful for CI gates |
| `restore` | Roadmap only from an existing portrait spec |

## The 5 Tiers

| Tier | Name | Total Score | What the portrait looks like |
|------|------|-------------|------------------------------|
| T1 | Veil | < 1.0 | Translucent child-shadow, faint waver |
| T2 | Shade | 1.0–2.5 | Half-formed humanoid, light cloak |
| T3 | Wraith | 2.5–5.0 | Armored revenant, visible aura |
| T4 | Revenant | 5.0–8.0 | Hulking cursed body, multiple wounds |
| T5 | Calamity | > 8.0 | Deity-class composite curse, environmental distortion |

> **Note on T5.** A Calamity portrait is alarming by design and is meant for budget conversations, not memes. Dorian will ask before publishing T5 outputs to a shared dashboard.

## What Dorian tracks

10 debt categories, each mapped to a visible portrait flaw:

| Category | Portrait surface |
|----------|------------------|
| Code Smells | Body distortion |
| Duplication | Doppelgängers, mirrored limbs |
| Cyclomatic Complexity | Extra arms, branching tendrils |
| Outdated Dependencies | Rusted armor, cracked weapons |
| Test Coverage Gap | Translucent / missing body parts |
| TODO / FIXME / HACK | Bandages, chains, gags |
| Architectural Violations | Tumors, conjoined growth |
| Security Debt | Toxic aura, dripping sigils |
| Performance Hotspots | Burning / freezing limbs |
| Documentation Gap | Eyeless face, sewn mouth |

Detection methods, thresholds, and tooling hints live in [`references/debt-detection.md`](references/debt-detection.md).

## Design principles

- **Detect honestly.** No flaw on the portrait without `path:line` evidence behind it.
- **Score before styling.** Tier comes from a formula, never a gut feel.
- **One flaw = one debt.** Strict 1:1 mapping is what makes Dorian a diagnostic instead of decorative AI art.
- **Restoration is the goal.** The portrait is a mirror; the roadmap is the point.
- **Mythic, not comedic.** Tone is gothic-serious. Humor undercuts the call to refactor.

## Privacy

Dorian scrubs the image prompt before output:

- File paths longer than the module name → redacted
- Function / class names → replaced with abstract roles ("the order processor")
- Customer / company / employee names → dropped
- Internal URLs, secrets, tokens, hashes → dropped

The image prompt should read as a piece of dark fantasy art with no clear connection to a specific company's source code.

## Repository layout

```
dorian/
├── SKILL.md                     # Main skill definition (~290 lines)
├── README.md                    # This file
└── references/
    ├── debt-detection.md        # 10 categories, detection methods
    ├── severity-rubric.md       # Tier scoring formula and weights
    ├── trait-mapping.md         # Findings → portrait flaws (1:1)
    ├── tier-codex.md            # Silhouette / palette / motif per tier
    ├── prompt-templates.md      # AI image prompt skeleton + ASCII fallback
    └── restoration-roadmap.md   # Flaw → refactor mapping rules
```

## License

MIT (suggested — set the LICENSE file to whatever you prefer).

## Acknowledgments

The portrait metaphor is borrowed from Oscar Wilde's *The Picture of Dorian Gray* (1890), now in the public domain.
