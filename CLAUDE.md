# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This repo *is* an agent skill package, not an app that gets built or tested. It contains no source
code, build system, or test suite — the "product" is the instruction text under `skills/`. There are
no build/lint/test commands to run; changes are validated by reading the instructions for internal
consistency, not by executing anything.

## Structure

```
.claude-plugin/plugin.json              # Claude Code plugin manifest; auto-discovers skills/ below
skills/github-feature-delivery/
├── SKILL.md                              # entry point: mode selection + shared invariants
├── agents/openai.yaml                    # Codex-specific packaging metadata (display name, default prompt)
└── references/
    ├── requirements-to-issues.md         # Mode 1: PRD/discussion -> GitHub Epic + sub-issues
    ├── issue-delivery.md                 # Mode 2: approved issue -> worktree -> implementation -> PR prep
    └── verification-and-pr.md            # Mode 3: branch/diff/PR -> acceptance review against an issue
```

The repo is packaged for two hosts today (Codex via `agents/openai.yaml`, Claude Code via
`.claude-plugin/plugin.json`, which needs no path config — Claude Code auto-scans `skills/*/SKILL.md`
under a plugin root). Adding a third host means adding one more packaging file/entry next to these,
never editing `SKILL.md`/`references/` to special-case a host — see the README's "Adding support for
another agent host" section.

`SKILL.md` is the router: it picks one of the three modes based on what the user handed it (an idea,
an approved issue, or a diff/PR), then defers to the matching file in `references/`. `SKILL.md` itself
carries invariants that apply across all three modes — evidence grounding, preserving user work,
one-issue-to-one-PR delivery units, and layered authorization (read-only vs. write vs. push/PR/merge)
— rather than repeating them in each reference file.

## Editing conventions for this skill

- **Provider neutrality**: the workflow and instructions in `SKILL.md` and `references/` must stay
  agent-host-neutral (no Codex-only or Claude-only assumptions baked into the instructions themselves).
  Host-specific packaging belongs outside those files — `agents/openai.yaml` for Codex,
  `.claude-plugin/plugin.json` for Claude Code — do not fold packaging concerns into the shared
  instructions, and do not add a third host's packaging without also adding it to "Agent compatibility"
  in the README once tested.
- **Authorization boundaries are load-bearing**: this skill's design intentionally separates read-only
  inspection, issue/label/relationship writes, code implementation, and commit/push/PR/merge/deploy into
  distinct authorization steps (see "Keep authorization granular" in `SKILL.md`). When editing any
  reference file, preserve this separation — don't collapse steps into an implicit "do everything" flow.
- **Decision gates over invented requirements**: the skill is designed to stop and file a decision issue
  when a choice would materially affect public behavior, data ownership, security, architecture, or
  acceptance criteria — rather than assume an answer. Keep this behavior when modifying
  `requirements-to-issues.md` or `issue-delivery.md`.
- The README's "Agent compatibility" table, "Skill structure", "Install in Codex", and "Install in
  Claude Code" sections must stay in sync with the actual layout under `skills/github-feature-delivery/`,
  `agents/openai.yaml`, and `.claude-plugin/plugin.json`. Only mark a host "tested" in that table after
  it has actually been installed and run end to end through that host — see the README's "Adding
  support for another agent host" section before changing a row's status.
