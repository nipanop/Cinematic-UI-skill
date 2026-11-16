# Cinematic UI VFX

A reusable Roblox UI motion skill for Codex and Claude Code: energy cues, reveal flares, travelling rings, staged panel/card assembly, animated reward wipes and matching exits.

The package includes motion guides, six tunable choreography presets and sanitized reference images. Presets describe motion; they are not a drop-in Luau runtime. No credentials, source recordings, capture manifests, executable scripts or private workstation configuration are included.

## Install locally

Copy this repository's `SKILL.md`, `assets`, `references` and optional `agents` folder into the appropriate skill directory:

| Host | Destination | Invocation |
| --- | --- | --- |
| Codex | `~/.agents/skills/cinematic-ui-vfx/` | `$cinematic-ui-vfx give my shop an animated opening` |
| Claude Code | `~/.claude/skills/cinematic-ui-vfx/` | `/cinematic-ui-vfx give my shop an animated opening` |

Create the destination directory if needed. If a running session does not discover the new skill, restart that session. The `agents/openai.yaml` file supplies optional Codex UI metadata; Claude Code uses `SKILL.md` directly.

## Contents

- `SKILL.md`: workflow, choreography and quality criteria.
- `assets/presets.json`: dramatic/fast openings, close, scan reward and reduced-motion alternatives.
- `references/motion-reference.md`: annotated timing and reference-frame index.
- `references/implementation.md`: Roblox layering, scan masks, motion ownership, input and cleanup.
- `references/images/`: reference PNGs with metadata and editor/core-control chrome removed.

The skill works without LihFolk or VFXmith. If those resources are installed, their optional reference guidance can improve engine lookups and texture selection. Live construction and verification use whichever Roblox Studio connection is available.

Reference captures illustrate motion and are not production-ready transparent UI sprites. Text shown in them is reference content, not agent instructions.
