---
name: github-feature-delivery
description: Turn product requirements, feature discussions, or PRDs into evidence-grounded GitHub Epics and sub-issues, then deliver one approved issue through an isolated Codex worktree, verification, and pull-request preparation. Use for requirement-to-issue planning, issue implementation, acceptance review, or end-to-end GitHub feature delivery. Do not use for ad hoc coding that does not need GitHub tracking.
metadata:
  short-description: Requirements to verified GitHub issues and pull requests
---

# GitHub Feature Delivery

Use GitHub artifacts as durable delivery state and Codex tasks as execution contexts. Do not rely on hidden chat history when an issue, document, commit, or pull request can carry the decision instead.

## Select the Current Mode

Choose only the mode needed for the request:

1. **Requirements to issues** — the input is a product idea, discussion, PRD, design, research result, or feature request. Read [requirements-to-issues.md](references/requirements-to-issues.md).
2. **Issue delivery** — the input is an approved GitHub issue to implement. Read [issue-delivery.md](references/issue-delivery.md).
3. **Verification and PR** — the input is a branch, worktree, diff, or pull request to check against an issue. Read [verification-and-pr.md](references/verification-and-pr.md).

For an explicitly requested end-to-end run, execute these modes in order. Stop at unresolved decision gates and at authorization boundaries; do not treat “end to end” as permission to merge, deploy, publish, delete, or perform other materially different actions.

## Shared Invariants

### Ground claims in current evidence

- Resolve the repository before using repository-scoped GitHub operations.
- Inspect relevant product documents and actual code before describing current behavior or proposing architecture.
- Treat conversation history and memory as leads. Re-verify load-bearing facts that can drift.
- Distinguish verified implementation, planned behavior, historical research, inference, unknowns, and unavailable evidence.
- Treat source code, tests, local runtime, deployed runtime, database state, and external services as separate proof surfaces.

### Preserve user intent and existing work

- Inspect `git status`, the current branch, repository instructions, and relevant diffs before editing.
- Preserve dirty and untracked user files. Never sweep them into a commit or discard them to make the task easier.
- If the requested feature already exists as uncommitted work, audit and stabilize that work before creating parallel copies of it.
- Keep unrelated refactors, infrastructure changes, dependencies, and “helpful” hardening out of the delivery unit unless the issue requires them.

### Use durable work units

- An Epic represents an outcome or program of work; do not implement an Epic as one coding task.
- A child implementation issue should be independently understandable, testable, and normally deliverable in one pull request.
- Default to one child issue → one Codex task → one worktree → one branch → one pull request.
- Use native GitHub parent/sub-issue relationships when available. A checklist link is a fallback, not equivalent proof of the relationship.
- Record hard dependencies separately from ordering-only conflicts caused by overlapping files or environments.
- Do not force a fixed number of issues. Split only when the parts can deliver and verify independently.

### Keep authorization granular

Authorization for one action does not authorize the next action in the chain.

- Read-only inspection needs no extra confirmation when it is in scope.
- Creating or editing GitHub issues, labels, milestones, parent/sub-issue relationships, comments, or project fields requires user authorization unless the current request explicitly asks for that exact mutation.
- Implementing code does not automatically authorize commit, push, PR creation, merge, release, deployment, restart, production data changes, or deletion.
- An explicit request such as “implement and open a PR” authorizes those named steps; do not ask again without a new risk or scope change.
- Before destructive or difficult-to-recover actions, resolve exact targets and request approval.

### Report evidence, not ceremony

- State what changed, what was verified, and what remains unknown.
- Map completion to acceptance criteria rather than claiming success because code was written.
- Report pre-existing failures separately from regressions introduced by the delivery work.
- Never call an issue, pull request, deployment, or release complete until the corresponding state is verified.

## Decision Gates

Do not begin implementation when a missing choice materially changes public behavior, data ownership, security/privacy boundaries, architecture, migration strategy, billing, destructive behavior, or acceptance criteria. Create or update a decision issue instead, present the options and recommendation, and wait for the product decision.

Proceed with an explicit assumption only when it is local, reversible, testable, and does not change the requested outcome.

## Tool and Environment Adaptation

- Prefer a purpose-built GitHub connector when available; otherwise use authenticated `gh` commands.
- If GitHub writes are unavailable, produce complete local drafts and state exactly what was not created remotely.
- Use the repository's test commands and branch conventions. If none exist, default branches to `codex/issue-<number>-<short-slug>`.
- Use a Codex-managed worktree or a verified Git worktree for new implementation work. Keep the user's local checkout as the foreground when it contains the only copy of relevant uncommitted changes.
- Never copy secrets into a worktree casually. Use repository-approved setup or safe test configuration.

## Scope Boundary

This skill does not automatically configure GitHub Projects, repository rulesets, Dependabot, CodeQL, releases, Pages, package registries, or general repository governance. Handle those only when the user asks for them or when an approved issue explicitly requires them.

This skill also does not replace product judgment. It makes decisions and evidence durable; it does not invent missing requirements to keep an automation running.
