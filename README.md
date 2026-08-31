# GitHub Feature Delivery Skill

An open-source, provider-neutral agent skill for turning product requirements, feature discussions, and PRDs into detailed GitHub Epics and sub-issues, then delivering one approved issue through an isolated worktree, tests, independent review, and pull-request preparation.

The skill is designed for teams and solo builders who want a durable workflow:

```text
Requirement → GitHub Epic / sub-issues → decision gates
→ isolated agent implementation → acceptance review → pull request
```

## Why this skill

- Embeds relevant document requirements inside each child issue.
- Verifies product claims against current code before filing or implementing work.
- Uses native GitHub parent/sub-issue relationships when available.
- Treats one implementation issue as one agent session, worktree, branch, and pull request.
- Separates hard dependencies from ordering-only conflicts.
- Preserves dirty worktrees and unrelated user changes.
- Keeps Issue edits, commits, pushes, PRs, merges, releases, and deployments behind distinct authorization boundaries.
- Reviews delivery against acceptance scenarios instead of equating code changes with completion.

## Agent compatibility

The skill is built on [Agent Skills](https://agentskills.io) — the open `SKILL.md` standard (a folder with a `SKILL.md` file: YAML frontmatter `name`/`description`, plus a Markdown body) that Claude Code, Codex, Cursor, Windsurf, Gemini CLI, GitHub Copilot, VS Code, and 30+ other agents all read natively. That means `skills/github-feature-delivery/` works as-is on any of them; only hosts with their own installer or plugin system get an extra thin packaging file so installation is one command instead of a manual copy.

| Host | Status | Project path | User/global path |
|---|---|---|---|
| Codex | tested, one-command install | `.agents/skills/` | `~/.agents/skills/` |
| Claude Code | tested, one-command install | `.claude/skills/` | `~/.claude/skills/` |
| Cursor | standard-compatible, untested | `.cursor/skills/` | `~/.cursor/skills/` |
| Windsurf | standard-compatible, untested | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` |
| Gemini CLI | standard-compatible, untested | `.gemini/skills/` | `~/.gemini/skills/` |
| GitHub Copilot | standard-compatible, untested | `.github/skills/` | `~/.copilot/skills/` |
| Any other Agent Skills client | standard-compatible, untested | see that host's docs | see that host's docs |

"Tested" means someone has actually installed and run this skill through that host end to end; "standard-compatible" means the host's own `SKILL.md` support should make it work, but no one has verified it here yet. Move a row from standard-compatible to tested by trying it and opening a PR — see [Adding support for another agent host](#adding-support-for-another-agent-host).

## Install in Codex

Ask the built-in skill installer:

```text
Use $skill-installer to install https://github.com/merlinz165/github-feature-delivery-skill/tree/main/skills/github-feature-delivery
```

The installed invocation is:

```text
$github-feature-delivery
```

## Install in Claude Code

This repository is a Claude Code plugin (`.claude-plugin/plugin.json` at the repo root), so it can be added directly from a local clone or a Git remote:

```text
/plugin marketplace add merlinz165/github-feature-delivery-skill
/plugin install github-feature-delivery
```

Or, without the plugin system, copy or symlink the skill directly into a skills directory Claude Code already scans:

```bash
# project-scoped
ln -s "$(pwd)/skills/github-feature-delivery" .claude/skills/github-feature-delivery
# user-scoped (all projects)
ln -s "$(pwd)/skills/github-feature-delivery" ~/.claude/skills/github-feature-delivery
```

Claude Code loads the skill by its `SKILL.md` frontmatter (`name`/`description`) and invokes it automatically when a request matches — there is no separate `$github-feature-delivery` invocation syntax like Codex's.

## Install in any other Agent Skills-compatible host

No installer or plugin manifest exists for these yet, but the skill folder is self-contained and follows the standard `SKILL.md` format, so a manual copy is normally enough:

```bash
# replace <project-path> with the row from the compatibility table above
cp -r skills/github-feature-delivery <project-path>/github-feature-delivery
```

Then invoke it the way that host normally triggers a skill (a slash command, an `@skill` mention, or automatic matching against the request — see that host's docs). If it works, please open a PR updating the compatibility table and, if the host has its own installer, adding a packaging file per [Adding support for another agent host](#adding-support-for-another-agent-host).

## Example prompts

```text
$github-feature-delivery Turn this feature discussion into a GitHub Epic and implementation-ready sub-issues. Draft first; do not write GitHub yet.
```

```text
$github-feature-delivery Implement GitHub issue #42 in an isolated worktree. Run the relevant tests, but do not push or open a PR.
```

```text
$github-feature-delivery Review PR #51 against issue #42. Stay read-only and report an acceptance matrix.
```

## Skill structure

```text
.claude-plugin/plugin.json          # Claude Code plugin manifest (auto-discovers skills/ below)
skills/github-feature-delivery/
├── SKILL.md                        # provider-neutral entry point: mode selection + shared invariants
├── agents/openai.yaml              # Codex-specific packaging metadata
└── references/
    ├── requirements-to-issues.md
    ├── issue-delivery.md
    └── verification-and-pr.md
```

The first release is instruction-only. It does not install a GitHub App, create GitHub Actions workflows, or bundle credentials.

## Adding support for another agent host

`SKILL.md` and everything under `references/` are the single, provider-neutral source of truth — the workflow, decision gates, and authorization boundaries must not vary by host. Moving a host from standard-compatible to tested means adding a thin packaging layer next to that source of truth, not forking it:

1. Verify it end to end: install the skill into that host (manual copy is enough — see above) and actually run all three modes against a real repository.
2. If the host has its own installer or plugin/marketplace system, add one self-contained packaging entry for it — for example `agents/<host>.yaml` for a Codex-style metadata file, or a manifest like `.claude-plugin/plugin.json` for Claude Code's plugin system — that points at `skills/github-feature-delivery/` without duplicating its content. If the host only needs a plain file copy, no packaging file is required.
3. Update the compatibility table and add an "Install in `<host>`" section to this README with real, tested commands.
4. Do not add host-specific conditionals inside `SKILL.md` or `references/`. If a host genuinely cannot support an invariant (for example, no isolated worktree equivalent), that is a compatibility limitation to document, not a fork point in the shared instructions.

## Design influences

This project was informed by patterns in:

- [OpenAI Skills](https://github.com/openai/skills)
- [richkuo/rk-skills](https://github.com/richkuo/rk-skills)
- [baphuongna/pi-crew](https://github.com/baphuongna/pi-crew)
- [Agent Skills specification](https://github.com/agentskills/agentskills) and the per-host compatibility-table convention used by community collections such as [seb1n/awesome-ai-agent-skills](https://github.com/seb1n/awesome-ai-agent-skills)

It is a separate, provider-neutral implementation with different scope and authorization boundaries.

## License

MIT
