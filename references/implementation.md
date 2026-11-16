# Building the choreography in Roblox

## Presentation controller and layer contract

Use the current UI framework. Keep one controller for a screen's desired state and transition ownership; keep reusable effect primitives separate from its gameplay data. A useful split is `MotionTokens`, `UIEffects`, and a panel/reward controller. These names are examples, not a required rewrite.

| Layer | Rendering approach | Responsibilities |
| --- | --- | --- |
| Focus/background | Non-interactive fullscreen art ScreenGui | Dim/tint, optional art, large glow/ring/flare; `ScreenInsets.None` for edge-to-edge art |
| Safe-area shell | Normal ScreenGui and motion wrapper | Root panel, header, controls; `CoreUISafeInsets` for interactive contents |
| Perimeter effects | Non-interactive ImageLabels/frame segments | Ring segments, arcs, particles behind or around content |
| Panel content | Native layout plus per-item motion wrappers | Staged heading/cards/actions without disturbing resting layout |
| Foreground reveal | Safe-area reveal root above panel | Full-size card/icon, mask, scan edge, result text, continue action |

Put one device scaler on the layout branch and motion scale on a descendant wrapper. Centre scale animations with `AnchorPoint = Vector2.new(0.5, 0.5)`. Pops animate `UIScale.Scale`, not a layout child's `Size`. Small temporary `Rotation` changes can communicate a shell settling; keep the final interactive panel level. Match active z/display order so particles cannot cover price or continue controls.

## Concrete effect primitives

### Swirl / energy cue

Use an asymmetric transparent swirl sprite or a few distinct arc segments. Start around the screen's focal centre, rotate while its diameter and coloured halo build. Use a hot small centre and a darker saturated outer body. Avoid three identical circles all growing at the same speed. Retain the swirl into the reveal hit, then let it recede behind the shell.

A ScreenGui recreation can be sufficient even when the reference's source might have been 3D. Build true 3D layers only when the requested camera/world interaction needs them, using the `vfx` workflow. No need to attach a UI opening to the avatar merely because the recording shows an avatar under its centre.

### Horizontal reveal flare

Build a thin bright centre bar plus a wider, lower-opacity coloured glow layer and a small core/glint. Grow length quickly and decay opacity/height; screen width can be the initial footprint for a dramatic modal. Use a transparent gradient sprite for soft falloff, or several inset native frames as a simpler prototype. GUI glow is alpha compositing; ParticleEmitter Brightness/LightEmission and bloom are not ImageLabel properties.

Keep the sharp bar distinct from its halo. Aim for a single decisive peak connected to shell arrival, not repeated bright flashing through the whole entrance. Optional accent lines should share the focal point.

### Travelling ring / perimeter arcs

Grow a ring from a smaller diameter using fast easing-out while fading. Maintain visible edge contrast early, then let it thin/disappear; do not leave a fixed gray disc over the menu. For electricity, author jagged arc sprites or native line segments around the rim, with sparse timed opacity changes. Confine persistent arcs to the perimeter so labels remain calm.

Keep world-effect library sizes separate: determine GUI diameters in panel-width or viewport units. A proposed short-lived ring can grow from 0.2 to 1.3 panel widths; tune against the captured result.

### Staged panel and cards

Reveal the shell/header first, settle its scale/rotation, and stagger cards through their child motion wrappers. Typical starting scale is 0.65–0.85 for cards, with a small single overshoot to 1.02–1.05 before 1.0. Stable layout positions exist before motion starts. Bound total stagger around 0.3–0.4 s for ordinary menus; large lists animate only the visible initial batch.

Do not hold all controls disabled while the decorative arrival finishes. Set focus once the panel is visible; respect close/back during every beat. If the caller needs a faster utility menu, shorten the cue and simplify the hit rather than retaining a long wait.

### Bottom-to-top scan reveal

Create a dim full-size copy of the card artwork behind a clipped bright copy. The bright artwork keeps its original size and alignment while a viewport/mask grows upward from the bottom. Move a separate luminous edge at the same normalized progress. The artwork must be revealed, not squeezed taller as the mask grows.

One implementation uses a bottom-anchored `ClipsDescendants` frame whose height goes from zero to the card height; the bright ImageLabel inside remains at the original card size, anchored to the bottom. Use an unrotated reveal wrapper for clipping and test the actual result. Keep the glow/scan bar outside the clipping branch so its halo is visible. Another option is an authored alpha-gradient wipe; verify it does not hide borders/text unexpectedly.

At scan completion, show the full art, decay the scan bar, then accent the payoff and reveal result labels. Preserve a readable result hold with explicit continue input. The reference's multi-second presentation is appropriate for a deliberate reward, not ordinary every-time shop opening.

### UI particle burst / tail

Pool small transparent sparkle, shard, leaf or currency sprites. Seed random positions/velocity for reproducible previews. Use one update loop for active particles, based on elapsed seconds rather than rendered frame count. Native positions may follow `p(t) = p0 + v0*t + 0.5*a*t*t`; scale/opacity resolve by normalized age. Release sprites after their lifetime and disconnect updates when no active effects remain.

For the initial version, a proposed 8–16 transient particles and only a few quiet idle specks can be enough; these are tuning choices, not Roblox limits. Measure overdraw and frame time before increasing them. Prefer a few clear shapes over many low-opacity dots. Text/currency/rarity information stays native UI.

## Timeline data and interruption

`../assets/presets.json` supplies semantic beats, with start/duration, effect kind, relative sizing and purpose. It is a design preset schema, not a drop-in Luau runtime. Convert it into the game's motion module and effects registry. Preload assigned image assets through the existing loader so a timed burst does not appear late while images fetch. If an asset is missing, use an intentional simpler primitive and disclose the visual gap.

Maintain `desiredOpen`, a transition generation/token, active tweens, delayed work and transient effects. On a new intent:

1. Increment the generation before cancelling previous work, since cancellation can synchronously fire completion handlers.
2. Cancel incompatible tweens and scheduled emissions; retarget the shell from its current state instead of resetting its scale abruptly.
3. Stop transient effects owned by the superseded transition; re-use surviving layers where continuity improves the result.
4. Animate toward the latest intent. A hide/destroy completion checks both `Enum.PlaybackState.Completed` and the generation/current desired state.

Generation checks also apply to delayed card arrivals, particle bursts, focus callbacks and reward returns. Avoid uncancellable `task.delay` chains that blindly alter state. Proportionally shorten travel when little distance remains. Do not replay a full anticipation on every interruption.

Reward sequences additionally track the presented result identity. Apply confirmed UI data once; presentation cancellation must not repeat a grant or lose acknowledged state. Define whether close cancels, minimizes or queues an active reveal according to the existing game. This skill does not authorize or design purchase/remotes logic.

## Comfort, input and performance

Use `Activated` and support close/back from mouse, touch and gamepad. Decorative full-screen layers are non-interactive; any modal input blocker is explicit, separate and removed on dismissal. Preserve the safe-area controls when decorative effects travel beyond screen edges.

Honour `GuiService.ReducedMotionEnabled` and the game's effects/intensity preference. Reduced mode substitutes short fades/colour changes, a localized accent and a static readable result; no vortex travel, shake or large white flash. Keep camera effects optional and under the existing camera owner. Screen-opening spectacle should work without camera shake.

Stop idle arcs, particles and shimmer when hidden. Avoid one render connection per spark or card, unbounded particle bursts, or full-screen translucent layers stacked indefinitely. Do not tween text outline thickness to pulse a title; scale a wrapper or tint the text instead.

## Capture and acceptance

Allow a deterministic preview mode: play a seeded sequence, restart it, and inspect named beats or sample a time. This makes before/after comparison useful rather than relying on random particles. Capture actual Studio play, not an HTML recreation claimed to be Roblox.

Inspect frames before/at/after the reveal hit, mid-card assembly, quiet hold, mid-close and returned gameplay. Record rapid open/close/reopen during the cue, shell arrival and stagger; final visibility must match the last command with no leftover blockers. Test low-frame-rate playback and a compact landscape phone. Verify reduced motion can change while a screen is open.

Report what is actually tested. A beautiful hero frame alone cannot establish timing, cancellation or phone performance.

## Local references and texture search

Optional LihFolk vault notes **UI Motion Language and Timing** (Interruption and Checklist), **UI and GUI Animation**, **Safe Areas and Insets**, and **Responsive UI for Phone Tablet Console and PC** inform the implementation. Read exact engine sections when using new APIs; the example ranges here are design defaults.

If VFXmith is installed, resolve the following paths relative to its repository root from the current project configuration. Read `knowledge/builder-notes.md` for crisp hero shapes, separate coloured glow, travelling rings and controlled aftermath. See `knowledge/categories/misc.md`, section `fea421530c94 Gravity/Pull`, and its matching library card/gallery for swirl/burst anatomy; do not transplant its 52-emitter world effect into GUI.

For reusable texture candidates:

Use the installed VFXmith texture helper `kit/scripts/find_textures.py` with roles such as `swirl=asymmetric magic swirl`, `ring=thin shockwave ring`, `flare=horizontal glow streak` and `spark=star sparkle`. Invoke it with the project's configured Python interpreter and save the candidate sheet to a chosen local output file. No executable, repository or output path is assumed by this skill.

Inspect the returned sheet before picking IDs; verify the selected texture is accessible and looks right as a GUI sprite. For flipbooks, use the helper's `--flipbook` mode and implement GUI atlas animation with the proper image rects if chosen. Particle Flipbook properties do not operate on ImageLabels. Use local assets/registry entries from the user's project when those already fit.
