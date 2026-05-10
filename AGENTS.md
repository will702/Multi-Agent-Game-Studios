# Codex Adapter for Claude Code Game Studios

This repository stores the game-studio agent framework in `.claude/`. Treat that
folder as the canonical source of truth even when working from Codex.

## Load Order

1. Read `CLAUDE.md` for project-wide context, collaboration rules, and imports.
2. Read `.claude/docs/coordination-rules.md` before any multi-agent or review
   workflow.
3. For a named agent, read `.claude/agents/<agent-name>.md` and use it as the
   role prompt.
4. For a slash-style request such as `/team-ui`, read
   `.claude/skills/team-ui/SKILL.md` and follow that workflow.

## Tool Translation

Claude-specific tool names in the source prompts map to Codex capabilities:

- `Task`: use Codex subagents when available. If subagents are not appropriate,
  run the named agent roles as explicit sequential review passes.
- `AskUserQuestion`: ask the user a concise structured question, using the
  available native input tool when present.
- `TodoWrite`: use Codex's native task plan/checklist mechanism when useful.
- `Read`, `Glob`, `Grep`, `Bash`, `Write`, `Edit`, `WebSearch`: use the nearest
  available Codex file, shell, edit, and search tools.

## Multi-Agent Rules

- Preserve the studio hierarchy: directors delegate to leads; leads delegate to
  specialists.
- Spawn or simulate multiple agents only when their workstreams are independent.
- For `team-*` skills, follow the skill's listed team composition and phases.
- If a skill requires parallel review and Codex subagents are unavailable, run
  the same roles one after another and report that the execution was sequential.
- Keep the user-driven protocol from `CLAUDE.md`: questions, options, user
  decision, draft, approval.

## Source Boundaries

- Do not copy `.claude/agents` or `.claude/skills` into a Codex-specific folder.
- Do not rewrite Claude-specific source prompts unless the requested change is
  meant to update the canonical framework.
- When adapting behavior for Codex, document the adapter behavior here or in
  `docs/MULTI_AGENT_AVAILABILITY.md`.
