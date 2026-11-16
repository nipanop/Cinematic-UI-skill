# Motion reference: focus on choreography

Reference A: an electric store transition, 976 x 480, approximately 32.283 s, nominal 60 fps. Reference B: a woodland menu transition, 1280 x 720, approximately 38.827 s, nominal 60 fps. Only sanitized reference frames are included; original recordings and capture manifests are excluded.

## Capture index

| Image | Sample range | Purpose |
| --- | --- | --- |
| [images/limited-opening.png](images/limited-opening.png) | 0.0–1.9 s, 0.1 s steps | Vortex, flare, expanding shell, staged cards and settled perimeter arcs |
| [images/limited-closing.png](images/limited-closing.png) | 30.0–31.9 s, 0.1 s steps | Cards withdraw, shell tilts/retracts, flare/ring clears, gameplay returns |
| [images/limited-reward.png](images/limited-reward.png) | 4.5–9.25 s, 0.25 s steps | Selected card foreground, bottom-to-top wipe, payoff, labels, return |
| [images/flare-hero.png](images/flare-hero.png) | 0.60 s | Bright thin horizontal flare, hot centre, violet ring behind emerging panel |
| [images/scan-hero.png](images/scan-hero.png) | 6.50 s | Bright reveal boundary, fully coloured lower artwork, dark upper artwork |
| [images/settled-hero.png](images/settled-hero.png) | 1.40 s | Fully assembled shop, quieter outline electricity and card glows |
| [images/woodland-opening.png](images/woodland-opening.png) | 7.50–8.875 s, 0.125 s steps | Different theme: interior blocks, rim, scenic trees, rings, leaves, sign and cards assemble |
| [images/woodland-reward.png](images/woodland-reward.png) | 24.0–28.5 s, 0.5 s steps | Foreground tree, currency/clover burst, gain number, restoration |

Read sheets left to right, then down. Sample times are requested video timestamps, with frame rounding near the source's nominal 60 fps. They establish visible sequence and approximate duration, not exact TweenInfo values.

For a new reference, inspect locally selected video frames at regular intervals using an available video tool. Keep source paths, account details and editor chrome out of shared captures. This package includes no executable capture helper or source-video manifests.

## Limited store entrance, observed

| Approximate clip time | Visible beat | Lesson |
| --- | --- | --- |
| 0.00–0.30 | Violet curved segments rotate around a brightening centre; gameplay is progressively darkened/tinted | Establish a motif and focal point before the shell arrives |
| 0.30–0.60 | The swirl expands, centre brightens, then a broad thin horizontal flare crosses the screen | Give the reveal a sharp directional hit rather than just a fade |
| 0.60–0.90 | Header and dark shell enlarge from the centre with a slight tilt; broad energy rings recede behind | Shell arrival is a distinct beat; the backdrop and flare provide continuity |
| 0.90–1.30 | Header text/decoration settle; three item cards appear/grow in sequence | Hierarchy assembles progressively without leaving a long empty panel |
| 1.30 onward | Cards are readable; purple perimeter arcs and subtle colour glow remain | Lower effect intensity once the interactive screen is established |

The recording shows a nearly screen-wide flare while the panel occupies approximately half the width. Match the relative footprint rather than copying fixed recording pixels. The opening uses stronger effects than the quiet hold. The apparent vortex centre overlaps the avatar in the recording, but the clip alone does not establish whether those layers are GUI, camera-space geometry or world emitters.

## Limited store exit, observed

Close begins around 30.5 s. Content collapses in stages, leaving the dark shell/header. The shell briefly tilts/shifts as the broad violet ring comes forward; it disappears into a horizontal flare around 31.1 s. Ring/flare decay and gameplay visibility returns by roughly 31.5 s.

Use this as inspiration for an intentional exit, not a requirement to reverse every entrance frame literally. Cancel remaining card arrivals before retracting them. Restore the actual underlying UI state and dim level, not a hard-coded scene.

## Reward scan reveal, observed

Around 4.75–5.25 s, a selected item card becomes a larger foreground object and the store behind darkens. The main card remains centred with visible dim artwork. A bright blue horizontal edge begins near its bottom and travels upward around 5.50–7.00 s; the image below becomes vivid blue while the image above stays dark. Small blue/white specks and a soft halo track the reveal without hiding its outline.

After the scan completes there is a short readable full-card hold. Around 7.6–8.0 s, a stronger white/blue hit accents the payoff and the result heading/name/edition text appear. A continue/return follows; the normal store has an owned state afterward. Other purchases repeat this choreography with magenta and gold rather than blue.

The clip demonstrates a strong bright flash. In a new game expose intensity/reduced-motion choices and favour a localized coloured hit when a full-screen white-out is unnecessary. The reveal should celebrate a confirmed result, not grant a purchase or fabricate an edition count.

## Woodland entrance and reward, observed

The earlier clip shares the same motion grammar despite very different art. Small interior sections enlarge, surrounding trees/rim/ground assemble, rings and streaks briefly punctuate the arrival, leaves cross the composition, then hero art and actions fill in. A later tree reward becomes the foreground focal object over a dimmed menu, bursts into acorns/clovers, shows a large gain label and returns to the ordinary screen.

Reuse the grammar: materialize the screen in layers, change focal ownership at each major beat, and let transient effects decay. Do not make woodland scenery mandatory for electric UI, or purple lightning mandatory for all screens.

## VFXmith reference and its limits

Looked at the local **Gravity/Pull** card `library/cards/misc/fea421530c94.md`, its gallery sheet `gallery/effects/misc/fea421530c94.jpg`, and the corresponding section in `knowledge/categories/misc.md`. The gallery shows a crisp magenta-white burst, swirling rings and small sparks with a short decay. Useful transfer: distinct hot core, saturated tails, outward/inward motion and fading secondary shapes.

That is a world-effect reference, not a ready UI transition. Its grey settling smoke, emitter count and stud-based speeds do not belong in a screen opener by default. The user's clips are the direct UI reference; VFXmith supports the effect design and texture search.
