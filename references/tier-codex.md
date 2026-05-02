# Tier Codex

Canonical silhouette, palette, and motif for each of the 5 tiers. Use during the `CHARACTERIZE` and `PROMPT` phases. The tier governs the base form; flaw surfaces from `trait-mapping.md` overlay it without replacing it.

The metaphor is a fall from grace. A low-debt codebase appears as a guardian angel — luminous, watchful, protective. As debt accumulates, the figure dims, kneels, fuses with armor and decay, and ultimately corrupts into an eldritch presence. The character is never replaced across tiers; it is the *same being*, falling.

| Arc keyword | Tier band | Mood |
|-------------|-----------|------|
| Grace | T1 | Luminous, hopeful, watchful |
| Fading | T2 | Tender, weary, halo dimming |
| Falling | T3 | Solemn, fused with steel and decay |
| Cursed | T4 | Dread, body horror beginning |
| Eldritch | T5 | Cosmic horror, terrain bending |

## T1 — Veil  (the Guardian)

- **Score**: < 1.0
- **Silhouette**: A small luminous figure, slightly shorter than a human; a pair of soft, well-kept wings; a faint halo above the head; light, simple robes
- **Posture**: Calm and watchful, hands resting open at the sides, gaze gentle and direct
- **Palette**: Warm ivory, dawn gold, soft cream, halo with a thin warm rim-light
- **Motif**: A single feather drifting at the figure's feet; the robe hem barely touching the ground; a small unlit lantern held loosely
- **Aura**: Soft warm glow, breath of fresh air around the figure
- **Environment**: Open, quiet sanctuary at first light; tall narrow windows; nothing threatening
- **Tone keywords**: luminous, watchful, hopeful, protective

> Read: a healthy young codebase. The portrait is a guardian — the team should feel that something good is being kept safe.

## T2 — Shade  (the Fading Angel)

- **Score**: 1.0–2.5
- **Silhouette**: Full-height humanoid; one wing tattered at the edge, the other still mostly intact; halo dimmed and slightly tilted; robe heavier, partially soiled at the hem
- **Posture**: Kneeling on one knee, or standing with one shoulder lower; the lantern is now lit but burning low
- **Palette**: Cool ivory turning to dusk grey, muted gold halo, charcoal seeping into the robe
- **Motif**: A single bandage on a forearm; a folded prayer or sealed scroll; the unlit feather at the feet has begun to grey
- **Aura**: Thin smoke or vapor pooling at the feet, dimming the warm glow
- **Environment**: Twilight cloister, indistinct ruins beginning at the periphery
- **Tone keywords**: tender, weary, dimming, watchful

> Read: an aging product with manageable debt. The portrait is an angel near the end of its watch — still recognizable, beginning to falter.

## T3 — Wraith  (the Fallen)

- **Score**: 2.5–5.0
- **Silhouette**: Armored revenant, ~1.1× human height; the wings have hardened into blackened blade-feathers; the halo is now a cracked iron ring; flesh and plate fused at the joints
- **Posture**: Combat-ready, rooted stance, one hand resting on a cracked sword; the figure has stopped asking permission
- **Palette**: Charcoal armor with verdigris, deep oxblood inner cloth, bone-white feather edges, halo iron-grey with cold light
- **Motif**: Cracked sword, sealed prayer scrolls turned to talismans, lantern relic now burning a pale green flame; battle-scarred bearing
- **Aura**: Visible heat-shimmer or cold embers, smoke trailing from old wounds, faint whispering pressure around the figure
- **Environment**: Battlefield ruin, eclipse-light, twilight bleeding into dusk
- **Tone keywords**: solemn, scarred, fallen, no-longer-asking

> Read: a working production system carrying real debt. The character has crossed from guardian to revenant — no longer a comfort.

## T4 — Revenant  (the Cursed Body)

- **Score**: 5.0–8.0
- **Silhouette**: Hulking cursed body, ~1.5× human height; asymmetric build; the wings are now ragged stumps or broken bone-frames; multiple mouths or wounds along the torso, breathing in unison; chains and seals trying and failing to contain the form
- **Posture**: Slouched but powerful, weight uneven; one limb visibly larger; the head bowed under its own weight, not in repentance
- **Palette**: Iron black, smoldering crimson cracks across the skin, sickly bile-yellow highlights, halo replaced by a dark broken crown
- **Motif**: Broken multiple weapons strapped to the body; heavy iron chains; multiple seals torn at the edges; parasitic growths along the spine
- **Aura**: Heavy aura distorting nearby air; visible miasma; the ground around the figure is cracked or weakly burning; faint thrum of something inside breathing
- **Environment**: Storm-lit ruin, heavy ash falling, the architecture itself failing in the background
- **Tone keywords**: cursed, restraint-failing, body-horror, dangerous

> Read: a system where debt has begun to bleed into reliability. The character should make the team feel urgency — not despair.

## T5 — Calamity  (the Eldritch)

- **Score**: > 8.0
- **Silhouette**: Eldritch composite, ~2–3× human height; multiple heads, many mouths, too many eyes to count; the wing-frames are now coiled tendrils; the halo is a ring of broken seals torn open; the original body is barely legible inside the mass
- **Posture**: Either enthroned or rising; the figure is the gravity well of the image — landscape collapses toward it
- **Palette**: Void black, molten core glow, ichor green or bone white accents, environmental color drained around the figure
- **Motif**: Crowns of broken seals, coiled chains being torn, multiple weapons fused into one weapon, parasitic forms emerging from the body
- **Aura**: A localized weather pattern — eternal storm, frozen sun, ash spiral; the world bends around it
- **Environment**: The figure *is* the environment; landscape is a husk; distance falls into the figure
- **Tone keywords**: civilization-scale, eldritch, dread, irreversible-without-coordinated-action

> Read: a system whose debt is now an existential business risk. **Confirm with the user before publishing.** The character is a deliberately unsettling diagnostic — useful for budget conversations, harmful as a meme.

## Cross-Tier Visual Continuity

The same character archetype falls across tiers — same silhouette family, same garment vocabulary, same color anchors that *invert* and *amplify* with each tier. When comparing two snapshots (e.g. before/after a refactor effort), hold the silhouette's identity constant (shoulder line, mask shape, weapon type, wing frame) and let the deformation, palette, and aura tell the change story.

The wing motif is the strongest continuity carrier:

| Tier | Wings |
|------|-------|
| T1 | Soft, well-kept, full plumage |
| T2 | One tattered, one intact; halo dimmed |
| T3 | Hardened into blackened blade-feathers |
| T4 | Ragged stumps, broken bone-frames |
| T5 | Coiled tendrils; the original wing is lost in the mass |

## Style Anchors (Image Generation)

These keywords go into every prompt regardless of tier:

- `painterly`, `dramatic chiaroscuro lighting`
- `single character, full body, three-quarter view`
- `painted concept art`
- `centered composition, neutral environment`, `clean background`

Tier-specific anchor band:

| Tier band | Anchors |
|-----------|---------|
| T1–T2 | `celestial guardian`, `religious painting tradition`, `warm rim-light`, `ethereal negative space`, `classical fantasy painting` |
| T3 | `dark heroic fantasy illustration`, `gothic chiaroscuro`, `armored revenant`, `cold ember lighting` |
| T4–T5 | `gothic horror dark portrait`, `body horror`, `oppressive atmosphere`, `eldritch dread`, `dense detailed linework` |

Avoid (always):

- modern brand cues, photographs, generic anime moe styling, cartoon comedy
- shock-gore (severed limbs, dismemberment for shock); never sexualized; never NSFW
- text or letters on the character (keeps the image localizable)

## Aspect Ratio and Resolution

- Default: `2:3` portrait (single-character full-body shot)
- For dashboards / banners: `16:9` may be requested by the user; pad with environmental haze rather than upscaling the figure
- Resolution: 1024×1536 baseline, 2048×3072 if the downstream image model supports it

## Tier Confirmation Checklist

Before composing the prompt, verify:

- [ ] Tier matches the formula in `severity-rubric.md` (no gut-feel override)
- [ ] Silhouette family fits the tier band (guardian / fading / fallen / cursed / eldritch)
- [ ] Palette anchors taken from the tier (not arbitrary)
- [ ] Wing/halo continuity reflects the tier
- [ ] Motif inventory drawn from this codex
- [ ] Flaw surfaces from `trait-mapping.md` overlay without contradicting the tier
