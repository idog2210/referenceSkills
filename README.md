# Claude Code Skills

Global Claude Code skills for UI/design work.

## Skills

- [`skills/reference-to-ui-exact`](skills/reference-to-ui-exact/SKILL.md) — rebuilds an existing screen or component from a reference image with measured visual fidelity, gated by an audit-only phase requiring explicit approval before any code change.
- [`skills/checking-design-fidelity`](skills/checking-design-fidelity/SKILL.md) — keeps styled UI edits consistent with a codebase's established design system (tokens, theme config, or reference components) using a Locate → Map → Verify → Flag procedure.
- [`skills/reusing-component-templates`](skills/reusing-component-templates/SKILL.md) — governs duplicating a component to hold new content: text is transported never authored, assets are re-declared never inherited, structure is copied never redesigned — ending in a required asset manifest and findings report.

## Usage

Copy a skill's folder into `~/.claude/skills/` (global) or `.claude/skills/` (project-scoped) to make it available to Claude Code.
