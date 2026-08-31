# Deliver One GitHub Issue

Use this mode only for an approved, implementation-ready child issue.

## 1. Validate the Work Unit

Before creating a worktree:

- read the complete issue and comments;
- confirm the issue is open and no active PR already implements it;
- read referenced repository documents and instructions;
- inspect the current code and tests supporting each load-bearing claim;
- identify unresolved decisions, hard dependencies, and ordering-only conflicts;
- note the issue's autonomy label if present (AFK or HITL) — for HITL, expect to pause at the flagged checkpoint rather than pushing through it;
- determine whether the issue can be delivered and verified in one PR.

If the issue is inaccurate but fixable, propose or apply an issue update according to the user's authorization. If it is materially ambiguous, too broad, already completed, or blocked by a decision, stop before implementation.

## 2. Inspect Git State

Check the current branch, default branch, worktrees, status, and relevant diffs.

- Preserve all unrelated dirty and untracked files.
- If the only relevant implementation exists as local uncommitted work, work from that state intentionally rather than pretending the remote default branch contains it.
- For new isolated work, start from the verified branch or commit containing all required predecessors.
- Do not create multiple worktrees for the same issue.

## 3. Create the Delivery Plan

Keep the plan proportional to risk. It should identify:

- files and layers expected to change;
- data or API compatibility considerations;
- test and verification points;
- documentation updates;
- explicit non-goals;
- the point at which user input or extra authority is required.

Use the issue's acceptance scenarios as the plan's completion checks. Do not add speculative hardening or unrelated cleanup.

## 4. Work in Isolation

For a Git repository, prefer an agent-managed or otherwise verified Git worktree. Respect repository branch conventions; otherwise use:

```text
feature/issue-<number>-<short-slug>
```

Verify that the worktree starts from the intended base before editing. If ignored local files are required, use repository-approved setup; do not broadly copy secrets or production credentials.

## 5. Implement the Issue

- Follow existing architecture, naming, error handling, and component conventions.
- Make the smallest coherent change that completely satisfies the approved issue.
- Update all required layers of the contract: schema, migration, repository, API, frontend, tests, docs, or runtime configuration only when the issue needs them.
- Preserve missing and unavailable states rather than fabricating defaults.
- Add regression or behavior tests that prove the acceptance criteria.
- Do not weaken tests just to make the tree green.

When implementation evidence contradicts the issue, stop or update the durable requirement. Do not silently deliver a different product behavior.

If implementation reveals the issue cannot actually close in one PR, stop and propose splitting the remainder into follow-up child issues rather than quietly expanding scope or forcing an oversized PR.

For a delivery long or interrupted enough to span multiple sessions, checkpoint progress on the issue itself (its checklist items or a progress comment) rather than only in local or ephemeral state, so any session resuming the work can read it back without the original conversation.

## 6. Verify Before Handoff

Run the repository's relevant checks. In proportion to risk, include:

- focused tests for the changed behavior;
- broader test suites affected by shared contracts;
- type checking, lint, formatting, and production build;
- migrations or seed/import validation;
- runtime, visual, device, or network checks when the issue requires them;
- `git diff --check` and a final `git status` review.

Record exact commands and outcomes. Distinguish failures introduced by the change from pre-existing failures. If a required check cannot run, label the coverage unavailable rather than passing it by inference.

## 7. Commit, Push, and PR Gates

Only perform the steps explicitly requested or already authorized:

1. Review the intended file list before staging.
2. Stage only issue-related files.
3. Commit with the repository's convention and the issue reference. If the repository has no established convention, default to Conventional Commits (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, ...) with the issue reference in the body.
4. Push the feature branch.
5. Create a PR against the verified default branch.

The PR should contain:

- concise outcome summary;
- acceptance scenarios covered;
- verification commands and results;
- known gaps or unavailable evidence;
- migration, rollout, or compatibility notes when relevant;
- `Closes #<number>` for the delivered child issue.

Do not merge, deploy, release, or delete the worktree unless those actions were requested.

## Completion Result

Report:

- worktree and branch;
- changed files and delivered behavior;
- acceptance criteria status;
- tests and other evidence;
- pre-existing or unresolved problems;
- commit and PR only if they actually exist;
- the next safe action.
