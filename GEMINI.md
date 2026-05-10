# Gemini Adapter for Claude Code Game Studios

This repository is a Claude Code Game Studios project, but Gemini should use the
same agents and skills through a repo-local adapter. The `.claude/` directory is
the canonical source of truth.

## Load Order

1. Read `CLAUDE.md` for the master game-studio context.
2. Read `.claude/docs/coordination-rules.md` before coordinating multiple roles.
3. For a named agent, read `.claude/agents/<agent-name>.md` and use it as the
   role prompt.
4. For a slash command request, map it to
   `.claude/skills/<skill-name>/SKILL.md`.

Examples:

- `/start` -> `.claude/skills/start/SKILL.md`
- `/team-ui inventory screen` -> `.claude/skills/team-ui/SKILL.md`
- `technical-director review` -> `.claude/agents/technical-director.md`

## Tool Translation

Claude-specific tool names in the source prompts map to Gemini behavior:

- `Task`: use Gemini's available delegation or multi-agent capability when
  present. Otherwise, run each named role as a clear sequential pass.
- `AskUserQuestion`: present a concise structured question in chat and wait for
  the user's decision.
- `TodoWrite`: maintain a visible checklist or plan if the task benefits from
  one.
- `Read`, `Glob`, `Grep`, `Bash`, `Write`, `Edit`, `WebSearch`: use equivalent
  Gemini file, shell, edit, and search capabilities.

## Operating Rules

- Keep `.claude` canonical; do not duplicate all agents and skills into
  `.gemini`.
- Preserve the collaboration protocol from `CLAUDE.md`: ask, present options,
  user decides, draft, approval.
- Preserve the studio hierarchy and escalation paths in
  `.claude/docs/coordination-rules.md`.
- When a skill asks for parallel agents and Gemini cannot run them in parallel,
  simulate the same roles sequentially and state that limitation in the result.
- When a source prompt references Claude Code hooks or statusline behavior,
  treat those as Claude-only automation unless an equivalent Gemini mechanism is
  explicitly configured.
