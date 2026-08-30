# Codex GitHub Feature Delivery Skill

An open-source Codex skill for turning product requirements, feature discussions, and PRDs into detailed GitHub Epics and sub-issues, then delivering one approved issue through an isolated worktree, tests, independent review, and pull-request preparation.

The skill is designed for teams and solo builders who want a durable workflow:

```text
Requirement → GitHub Epic / sub-issues → decision gates
→ Codex worktree implementation → acceptance review → pull request
```

## Why this skill

- Embeds relevant document requirements inside each child issue.
- Verifies product claims against current code before filing or implementing work.
- Uses native GitHub parent/sub-issue relationships when available.
- Treats one implementation issue as one Codex task, worktree, branch, and pull request.
- Separates hard dependencies from ordering-only conflicts.
- Preserves dirty worktrees and unrelated user changes.
- Keeps Issue edits, commits, pushes, PRs, merges, releases, and deployments behind distinct authorization boundaries.
- Reviews delivery against acceptance scenarios instead of equating code changes with completion.

## Install with Codex

Ask the built-in skill installer:

```text
Use $skill-installer to install https://github.com/merlinz165/codex-github-feature-delivery-skill/tree/main/skills/github-feature-delivery
```

The installed invocation is:

```text
$github-feature-delivery
```

## Example prompts

```text
$github-feature-delivery Turn this feature discussion into a GitHub Epic and implementation-ready sub-issues. Draft first; do not write GitHub yet.
```

```text
$github-feature-delivery Implement GitHub issue #42 in an isolated Codex worktree. Run the relevant tests, but do not push or open a PR.
```

```text
$github-feature-delivery Review PR #51 against issue #42. Stay read-only and report an acceptance matrix.
```

## Skill structure

```text
skills/github-feature-delivery/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── requirements-to-issues.md
    ├── issue-delivery.md
    └── verification-and-pr.md
```

The first release is instruction-only. It does not install a GitHub App, create GitHub Actions workflows, or bundle credentials.

## Design influences

This project was informed by patterns in:

- [OpenAI Skills](https://github.com/openai/skills)
- [richkuo/rk-skills](https://github.com/richkuo/rk-skills)
- [baphuongna/pi-crew](https://github.com/baphuongna/pi-crew)

It is a separate Codex-native implementation with different scope and authorization boundaries.

## License

MIT
