# Multi-Agent Availability for Gemini and Codex

Claude Code Game Studios now exposes repo-local entrypoints for Gemini and
Codex while keeping `.claude` as the single source of truth.

## Canonical Source

- Master project context: `CLAUDE.md`
- Agents: `.claude/agents/*.md`
- Skills: `.claude/skills/*/SKILL.md`
- Coordination rules: `.claude/docs/coordination-rules.md`
- Hooks and Claude Code automation: `.claude/settings.json` and `.claude/hooks/`

Do not duplicate the 49 agents or 72 skills into `.gemini` or `.codex` folders.
Adapters should point host tools back to the canonical files.

## Entrypoints

- Codex reads `AGENTS.md`.
- Gemini reads `GEMINI.md`.
- Claude Code continues to read `CLAUDE.md` and `.claude/settings.json`.

These adapter files explain how each host should map its native capabilities to
the existing Claude-oriented framework.

## Skill Routing

When a user invokes a slash-style skill, resolve it by name:

```text
/team-ui inventory screen
```

Read:

```text
.claude/skills/team-ui/SKILL.md
```

Then follow the skill phases, replacing Claude-only tool names with the host
agent's equivalent capabilities.

## Agent Routing

When a user invokes or references a named studio role, resolve it by name:

```text
technical-director review this architecture
```

Read:

```text
.claude/agents/technical-director.md
```

Use that markdown file as the role prompt. Preserve the agent's responsibilities,
delegation map, gate verdict format, and escalation boundaries.

## Tool Translation Table

| Claude source term | Gemini/Codex adapter behavior |
| --- | --- |
| `Task` | Use native subagents/delegation when available; otherwise run named roles sequentially. |
| `AskUserQuestion` | Ask a concise structured question using the host's available input method. |
| `TodoWrite` | Use the host's native plan/checklist mechanism, or maintain a visible checklist in chat. |
| `Read`, `Glob`, `Grep` | Use host file-read and search tools. |
| `Bash` | Use host shell execution with its normal approval/sandbox rules. |
| `Write`, `Edit` | Use host editing tools after following the collaboration protocol. |
| `WebSearch`, `WebFetch` | Use available web/documentation search tools when enabled. |

## Parallel and Team Workflows

For `team-*` skills, preserve the listed team composition and phase gates. If the
host supports real parallel subagents, dispatch independent roles together. If
it does not, run the same role passes sequentially and note that the workflow was
simulated sequentially.

Keep the coordination rules unchanged:

- Directors own cross-domain strategy.
- Leads own domain plans and handoffs.
- Specialists own implementation details inside their domain.
- Cross-domain conflicts escalate to the shared parent, or to
  `creative-director` / `technical-director` when no shared parent is clear.

## Claude-Only Automation

The following remain Claude Code-specific unless a future adapter adds native
support:

- `.claude/settings.json` hook events
- `.claude/statusline.sh`
- `SubagentStart` / `SubagentStop` audit hooks
- Claude Code permission allow/deny rules

Gemini and Codex should still respect the intent of those controls manually:
avoid destructive commands, preserve approval gates, and report when automation
is not available in the current host.
