# Prompt Templates

How Dorian composes the AI image-generation prompt during the `PROMPT` phase. The output is **model-agnostic**: pass it to Gemini, OpenAI, Stable Diffusion, Midjourney, or any other image model. Dorian's job is to compose a high-quality, evidence-grounded prompt — rendering itself is delegated to whatever the user has wired up.

The tone is keyed to the tier band — celestial for T1–T2, fallen-knight for T3, gothic horror for T4–T5. See `tier-codex.md` for the full arc.

## Master Prompt Skeleton

```
[Subject + tier silhouette]: <one-line description, including wing/halo state>
[Form details]: <silhouette family from tier-codex.md, posture, size>
[Flaw inventory]: <comma-separated flaw surfaces with body region tags>
[Aura and atmosphere]: <aura intensity from tier, environmental notes>
[Palette]: <tier palette anchors>
[Style anchors]: <tier-band anchors from tier-codex.md>, painterly, dramatic chiaroscuro, single character full body, three-quarter view, painted concept art, clean neutral background
[Composition]: 2:3 portrait, centered, full body
[Mood]: <three tone keywords from tier-codex.md>
[Negative prompt]: text, letters, modern brand, photograph, anime moe, cartoon comedy, shock gore, dismemberment, sexualization, NSFW, multiple characters, watermark, signature
```

## Worked Example A — T1 Veil  (the Guardian)

```yaml
character_prompt:
  positive_prompt: |
    A T1 Veil — a small luminous guardian, slightly shorter than human, full body, three-quarter view.
    Calm watchful posture, hands open at the sides, soft well-kept wings, faint halo above the head.
    Light cream robes, hem barely touching the ground, an unlit lantern held loosely.
    Flaw surfaces:
      - a single bandage on the wrist (TODO/FIXME, light intensity)
      - a faint patina on the lantern (outdated dependencies, light intensity)
    Palette: warm ivory, dawn gold, soft cream, halo with a thin warm rim-light.
    Atmosphere: open quiet sanctuary at first light, tall narrow windows, nothing threatening.
    Style: celestial guardian, religious painting tradition, warm rim-light, ethereal negative space,
    classical fantasy painting, painterly, dramatic chiaroscuro, painted concept art,
    single character full body, three-quarter view, clean neutral background.
    Mood: luminous, watchful, hopeful.
  negative_prompt: |
    text, letters, modern brand, photograph, anime moe styling, cartoon comedy,
    shock gore, dismemberment, sexualization, NSFW, multiple characters,
    watermark, signature, ui chrome
  style_anchors:
    - celestial guardian
    - religious painting tradition
    - warm rim-light
    - painterly
    - dramatic chiaroscuro lighting
    - single character full body
    - three-quarter view
  aspect_ratio: "2:3"
  resolution: "1024x1536"
  notes: "Healthy codebase. The portrait reads as a guardian; debt cues are present but light."
```

## Worked Example B — T3 Wraith  (the Fallen, security-dominant)

```yaml
character_prompt:
  positive_prompt: |
    A T3 Wraith — a fallen revenant, ~1.1× human height, full body, three-quarter view.
    Combat-ready stance, one hand on a cracked sword; wings hardened into blackened blade-feathers;
    cracked iron halo behind the head; flesh and plate fused at the joints.
    Flaw surfaces:
      - toxic green aura pooling around the legs (security debt, dominant)
      - rusted shoulder pauldron and cracked greaves (outdated dependencies)
      - third arm branching from the right shoulder holding a second blade (cyclomatic complexity)
      - translucent left hand, fingers half-faded (test coverage gap)
      - parchment seals tied around the right forearm (TODO/FIXME)
      - lower face wrapped in cloth, eyes covered by a veil (documentation gap)
    Palette: charcoal armor with verdigris, deep oxblood inner cloth, bone-white feather edges,
    halo iron-grey with cold light, sickly green aura.
    Atmosphere: battlefield ruin, eclipse-light, twilight bleeding into dusk; smoke trailing from old wounds.
    Style: dark heroic fantasy illustration, gothic chiaroscuro, armored revenant, cold ember lighting,
    painterly, dramatic chiaroscuro, painted concept art, single character full body, three-quarter view,
    clean neutral background.
    Mood: solemn, scarred, fallen.
  negative_prompt: |
    text, letters, modern brand, photograph, anime moe styling, cartoon comedy,
    shock gore, dismemberment, sexualization, NSFW, multiple characters,
    watermark, signature, ui chrome
  style_anchors:
    - dark heroic fantasy illustration
    - gothic chiaroscuro
    - armored revenant
    - painterly
    - dramatic chiaroscuro lighting
    - single character full body
    - three-quarter view
  aspect_ratio: "2:3"
  resolution: "1024x1536"
  notes: "Working production system carrying real debt. Wing motif fully blackened — visible fall from grace."
```

## Worked Example C — T5 Calamity  (the Eldritch)

```yaml
character_prompt:
  positive_prompt: |
    A T5 Calamity — eldritch composite, 2.5× human height, full body.
    Multiple heads, many mouths, too many eyes; coiled tendrils where wings once were;
    halo replaced by a ring of broken seals torn open; original body barely legible inside the mass.
    Posture rising, terrain folding toward the figure.
    Flaw surfaces:
      - heavy iron chains around the torso, multiple manacles (TODO/FIXME, severe)
      - tumorous mass conjoined to the back, second head emerging from the chest (architectural violations)
      - corroded armor, weapon snapped, parasitic worms in the cracks (outdated dependencies)
      - whole right limb engulfed in slow flame (performance hotspots)
      - full-body toxic miasma, sigils dripping ichor (security debt)
    Palette: void black, molten core glow, ichor green and bone white accents, environment color drained.
    Atmosphere: localized eternal storm, ash spiral; the landscape collapses toward the figure.
    Style: gothic horror dark portrait, body horror, oppressive atmosphere, eldritch dread,
    dense detailed linework, painterly, dramatic chiaroscuro, painted concept art,
    single character full body, three-quarter view.
    Mood: civilization-scale, eldritch, dread.
  negative_prompt: |
    text, letters, modern brand, photograph, anime moe styling, cartoon comedy,
    shock gore, dismemberment, sexualization, NSFW, multiple characters,
    watermark, signature, ui chrome
  style_anchors:
    - gothic horror dark portrait
    - body horror
    - oppressive atmosphere
    - eldritch dread
    - painterly
    - dramatic chiaroscuro lighting
    - single character full body
    - three-quarter view
  aspect_ratio: "2:3"
  resolution: "1024x1536"
  notes: "T5 — confirm with user before publishing. Imagery is deliberately unsettling."
```

## PII Scrub Pass

Before finalizing the prompt, run a final scan over it:

| Pattern | Action |
|---------|--------|
| File paths longer than the module name | Redact to module name only |
| Function or class names verbatim | Replace with abstract role ("the order processor") |
| Customer / company / employee names | Drop entirely |
| Internal URLs | Drop entirely |
| Proprietary product names | Replace with category label |
| Secrets, tokens, hashes (even partial) | Drop entirely; flag in the review |

The character prompt should read as a piece of dark fantasy / religious-painting-tradition art with no clear connection to a specific company's source code.

## Composition Variants

| Use case | Composition tweak |
|----------|-------------------|
| Retro slide deck | Default 2:3 portrait |
| README banner | 16:9; pad with environmental haze; figure on the left third |
| Onboarding doc inline | 1:1 square; tighten crop to mid-thigh up |
| Before/after diptych (run twice across two refs) | Two 2:3 portraits side-by-side, same lighting and seed family; hold silhouette identity constant |

## ASCII Fallback

If no image generator is wired up or the user requests text-only output, render the same character as ASCII art with a structured flaw list. Keep the silhouette family and tone — the textual character should still feel keyed to its tier band (guardian / fallen / cursed / eldritch).

```
       .--""--.
      /  ___   \
     | / · _\ · |    ← veiled face: documentation gap
     |  \_____/  |
     |  /     \  |    ← third arm: cyclomatic complexity
     |~~~~~~~~~~~|
     |  RUSTED   |    ← outdated dependencies
     |   ARMOR   |
     |~~~~~~~~~~~|
     |  >toxic<  |    ← security debt
     |   ~aura~  |
     '-_______-'

Tier:        T3 — Wraith   (Score 4.7)
Dominant:    Security debt
Flaws:       Toxic aura · Rusted armor · Third arm · Translucent hand
             · Parchment seals · Veiled face
```

## Reproducibility

For before/after comparisons (run Dorian against two refs):

1. Compute a stable seed hash: `hash(repo_url + ISO_date)`.
2. Use the same seed family across the diptych (left = old, right = new).
3. Hold style anchors and silhouette family identical; only let flaw surfaces, tier band, and palette change.
4. Note the seed in the review so future runs can reproduce the comparison.

## Output Packet

The final character prompt deliverable:

```yaml
character_prompt:
  run_id: <uuid>
  tier: T3
  total_score: 4.7
  positive_prompt: <string>
  negative_prompt: <string>
  style_anchors: [<list>]
  aspect_ratio: "2:3"
  resolution: "1024x1536"
  seed: <int>
  flaws: [<per-flaw entries with category, intensity, source_finding, citation, visual_note>]
  pii_scrub_result: passed
  notes: <optional, e.g. "T5 — confirm with user before publishing">
```
