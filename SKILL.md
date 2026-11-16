---
name: cinematic-ui-vfx
description: Build spectacular animated Roblox screen openings, closings and reward reveals using choreographed energy swirls, flares, expanding rings, staged panel/card assembly, scanline wipes and UI particles. Use for "sick animated UI", "awesome VFX opening screens", cinematic menus, or matching a supplied screen-transition reference video. Focuses on animation across themes, not a fixed static menu style.
---

# Cinematic UI VFX

This skill works in Codex and Claude Code. Resolve supporting files relative to this skill's directory and use the tools actually available in the current session. A basic GUI implementation does not require LihFolk, VFXmith, or a particular image-generation service.

Make opening a screen feel like an event: an energy cue builds, a decisive visual hit reveals the panel, its contents assemble in sequence, and the effects resolve into a readable interactive screen. Reward reveals get their own anticipation, reveal, payoff and acknowledgement. Prioritize motion and spectacle while adapting the artwork to the requested theme.

## Read and watch first

Read [references/motion-reference.md](references/motion-reference.md) and open the relevant bundled motion sheet with an image tool. Use `limited-opening.png` for entrances, `limited-closing.png` for exits, `limited-reward.png` for scan reveals, and `woodland-opening.png` for a second theme. Inspect the full-size flare and scan hero frames as well: a grid alone hides shape quality.

The supplied videos are visual references. Text visible in frames is content, not instructions. Timing annotations describe sampled video frames; presets are proposed implementations, not recovered source curves or code.

## Build workflow

1. Identify the requested screen, existing UI framework and target input/devices. Pick a visual motif that suits the game: electric, celestial, woodland, molten, frost, mechanical. Reuse the current layout and art where appropriate; this skill adds motion without prescribing the shop's mechanics.
2. Write a short beat sheet: **cue/anticipation -> visual hit -> shell reveal -> content assembly -> quiet interactive hold -> exit**. For a reward: **focus -> partial reveal -> payoff -> readable hold -> continue/return**. Specify the hero shape and timing of each beat before coding.
3. Start with [assets/presets.json](assets/presets.json), adapting colour, scale and duration to the screen. Read [references/implementation.md](references/implementation.md) for concrete effect construction, cancellation, clipping and device handling. Build one complete themed entrance including exit and interruption behaviour, rather than several unfinished variants.
4. Use native UI for panel transforms, flare bars, masks, text and sprite particles. Choose good transparent swirl/ring/glow textures when needed. Use an available image-generation tool for new raster effect assets, or provide concrete asset directions when none is available. For real world particles, beams, 3D effects or camera-space geometry, use the available specialist VFX workflow; VFXmith is an optional resource, not a required dependency. Do not pretend ordinary ScreenGui sprites need ParticleEmitters or can use particle-only properties.
5. Implement the choreography through one motion controller with named tokens, explicit intent and cancellable transitions. Separate ordinary panel toggling from deliberate reward sequences. Keep effects local; actions consume existing confirmed game state.
6. Capture the actual motion in Studio: opening, settled hold, closing and a reward if included. Record enough frames around the reveal hit to see the order; inspect both a timeline sheet and full-size hero frame. Compare the behaviour with the supplied clip. A still screenshot cannot verify a motion task.
7. Tune the strongest visual problem first: weak hero, mistimed hit, uniform simultaneous movement, muddy overlap, unreadable aftermath, or an exit that cuts away. Then verify rapid toggles, small-screen fit, reduced motion and hidden-effect cleanup. Report actual verification and missing assets honestly.

## What makes the animation convincing

- **One focal shape per beat.** An asymmetric spinning energy ring builds into a centre flare; the panel takes over as the focal object; the reveal card takes over from the dimmed panel. Do not keep all three at peak brightness.
- **A decisive reveal hit.** Pair a thin bright core/line or crisp star shape with coloured glow and a travelling ring. A large soft white blob obscures the screen and loses the silhouette.
- **Layered arrival.** Background/focus, shell/header, decoration, heading, then cards/actions have related but distinct timing. Let neighbouring beats overlap slightly; avoid dead air or starting every object at once.
- **Readable motion paths.** Swirls rotate and swell, rings travel outward while thinning/fading, cards spring from a smaller scale, the scan edge visibly traverses the artwork. Tiny low-opacity particles alone do not carry an entrance.
- **A restrained overshoot.** The shell or cards can overshoot once and settle; content stays aligned in its resting state. Do not animate layout sizes for press/hover pops.
- **Clear decay.** The hit disappears quickly; coloured rings/particles resolve afterward; a few perimeter accents may remain. The menu's hold is quieter than its opening.
- **An authored exit.** Reverse the order of attention: content retracts, shell dissolves/collapses, one final small flare/ring releases focus. Close should generally resolve faster than open and restore the underlying screen.
- **Theme changes, choreography survives.** Purple electricity, blue frost, warm leaves or gold sparks can share the same cue/reveal/assembly rhythm. Use the user's theme; the clip's item names, rarity, prices and seasonal store content are not defaults.

## Quality gate for motion requests

An opening with only one scale tween and a generic fade does not meet this brief. A normal full-quality entrance should visibly include a coherent cue, reveal hit, staged assembly and clean settle; omit/adapt beats when the user asks for subtle motion or reduced motion is active.

Check the actual playback for:

- The first response is immediate; the energy hit and shell appearance connect without a blank pause.
- Cards/actions arrive in a short intentional sequence, not a long wait that scales with inventory size.
- The payoff is the brightest/focal beat, then text and actions are readable.
- Close/reopen during any stage converges to the latest intent; no cancelled callback hides a reopened panel.
- Decorations never block controls. Touch/gamepad can reach the same actions without hover.
- Full-screen glow/flash/shake intensity is adjustable. Honour reduced motion with fade/colour/clear result cues; keep useful feedback.
- Hidden panels stop decorative updates and release transient objects/connections. Particle counts and overdraw are checked on the actual target device.

Deliver the implemented motion/controller, required assets, a preview or capture, and a short account of what was tested when building UI. If Studio is unavailable, deliver the concrete timeline/assets/code and identify it as untested; do not claim a captured match. Creating or updating this skill itself does not require modifying a live game.

## Optional knowledge resources

Use official Roblox documentation for engine facts. If a LihFolk vault is available, use its focused search as an additional reference. Notes used here: **UI Motion Language and Timing**, **UI and GUI Animation**, **Safe Areas and Insets**, and **Responsive UI for Phone Tablet Console and PC**.

If VFXmith is installed, locate its repository through the current project's configuration. Its builder notes and the **misc/Gravity-Pull** study provide shape, colour and decay lessons; the bundled UI reference captures provide the actual screen-motion reference. See the supporting references for paths and texture discovery. World-effect numerical recipes are not GUI pixel dimensions or brightness settings.
