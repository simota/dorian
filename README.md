# Dorian

> *"Every codebase has a portrait. Dorian shows you yours."*

Dorian is a self-contained [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that does two things:

1. **Reviews technical debt thoroughly** across 10 categories with severity scoring (T1 Veil → T5 Calamity).
2. **Generates an AI image-generation prompt** that anthropomorphizes the accumulated debt as a single character.

The character is *the same being across all tiers, falling*. A low-debt codebase reads as a luminous guardian angel — soft wings, faint halo, watchful posture. As debt accumulates, the figure dims, kneels, fuses with armor and decay, and finally corrupts into something eldritch. The silhouette doesn't change identity; the corruption changes the silhouette.

The metaphor is Oscar Wilde's *The Picture of Dorian Gray*: the codebase keeps shipping while a hidden portrait silently absorbs every shortcut, every aging dependency, every skipped test.

## What you get from one invocation

Each invocation returns a single bundle with two artifacts:

1. **Debt Review** — per-category scores, total score, tier (T1–T5), and the top findings with `path:line` evidence. Thorough enough to act on.
2. **Character Prompt** — model-agnostic positive / negative prompts plus style anchors. Pass it to Gemini, OpenAI, Stable Diffusion, Midjourney, or any image model. ASCII fallback when no image model is wired up.

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
- "Use Dorian to review technical debt and generate a character."
- "Run Dorian's audit-only mode against this branch."

## Modes

| Mode | When to use |
|------|-------------|
| default | Full bundle: debt review + character prompt |
| `audit-only` | Score only, no character — useful for CI gates |

## The 5 Tiers — a fall from grace

| Tier | Name | Total Score | Character |
|------|------|-------------|-----------|
| T1 | Veil | < 1.0 | A small luminous guardian — soft wings, faint halo, watchful |
| T2 | Shade | 1.0–2.5 | A fading angel — one wing tattered, halo dimmed, kneeling |
| T3 | Wraith | 2.5–5.0 | A fallen revenant — flesh and steel fused, wings now blackened blades |
| T4 | Revenant | 5.0–8.0 | Hulking cursed body — multiple mouths breathing in unison, miasma at the feet |
| T5 | Calamity | > 8.0 | Eldritch composite — many heads, many mouths, the terrain folding into it |

> **Note on T5.** A Calamity character is deliberately unsettling and is meant for budget conversations, not memes. Dorian will ask before publishing T5 outputs to a shared dashboard.

## What Dorian tracks

10 debt categories, each mapped to a visible character flaw:

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

Detection methods, thresholds, and tooling hints live in [`references/debt-detection.md`](references/debt-detection.md).

## Design principles

- **Detect honestly.** No flaw on the character without `path:line` evidence behind it.
- **Score before styling.** Tier comes from a formula, never a gut feel.
- **One flaw = one debt.** Strict 1:1 mapping is what makes Dorian a diagnostic instead of decorative AI art.
- **Same being, falling.** The character is the codebase across all tiers, corrupting — not five separate creatures.
- **Sinister, not shock.** T1–T2 are celestial; T3 is a falling moment; T4–T5 are gothic horror with body distortion. Never NSFW, never shock-gore, never sexualized.

## Privacy

Dorian scrubs the image prompt before output:

- File paths longer than the module name → redacted
- Function / class names → replaced with abstract roles ("the order processor")
- Customer / company / employee names → dropped
- Internal URLs, secrets, tokens, hashes → dropped

The image prompt should read as a piece of dark fantasy / religious-painting-tradition art with no clear connection to a specific company's source code.

## Repository layout

```
dorian/
├── SKILL.md                     # Main skill definition
├── README.md                    # This file
├── index.html                   # GitHub Pages landing page
├── assets/styles.css            # Landing page styles
└── references/
    ├── debt-detection.md        # 10 categories, detection methods
    ├── severity-rubric.md       # Tier scoring formula and weights
    ├── trait-mapping.md         # Findings → character flaws (1:1)
    ├── tier-codex.md            # Silhouette / palette / motif per tier
    └── prompt-templates.md      # AI image prompt skeleton + ASCII fallback
```

## License

MIT (suggested — set the LICENSE file to whatever you prefer).

## Acknowledgments

The portrait metaphor is borrowed from Oscar Wilde's *The Picture of Dorian Gray* (1890), now in the public domain.
