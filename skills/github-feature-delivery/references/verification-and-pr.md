# Verify Delivery Against an Issue

Use this mode for independent acceptance review of a branch, worktree, diff, commit, or pull request.

## Review Contract

Default to read-only. Do not repair findings unless the user separately asks for fixes.

Review the delivered artifact against:

1. the linked child issue and its latest comments;
2. referenced product and technical documents;
3. repository instructions and architecture boundaries;
4. the actual diff and affected call paths;
5. tests, builds, runtime evidence, and CI results.

Do not accept a PR summary as proof that the code implements it.

## 1. Resolve the Baseline

Identify:

- repository and default branch;
- reviewed head commit;
- base commit or base branch;
- linked Issue and PR;
- whether the head changed after tests or review evidence was produced.

Avoid reviewing ambiguous local state. When the working tree is dirty or divergent, use explicit commits/refs for the comparison.

## 2. Build an Acceptance Matrix

For every acceptance scenario, record one result:

- **Verified** — direct code, test, runtime, visual, or external-state evidence proves it.
- **Partially verified** — only part of the behavior or environment is covered.
- **Not verified** — no sufficient evidence was produced.
- **Failed** — evidence contradicts the required behavior.
- **Not applicable** — the issue explicitly excludes it.

Name the evidence for every result. Tests prove only what they actually assert.

## 3. Inspect Risk Surfaces

Follow changed contracts through relevant producers and consumers. Check as applicable:

- error, empty, loading, timeout, retry, cancellation, and unavailable states;
- concurrency, idempotency, migrations, rollback, and version compatibility;
- authentication, authorization, privacy, secrets, and regional data boundaries;
- mobile/desktop or other parity surfaces promised by the issue;
- recent changes to the same paths;
- documentation and configuration drift.

Do not expand into a general security or architecture audit unless the issue or diff creates that risk.

## 4. Run Proportionate Verification

Run or inspect the checks relevant to the diff. For UI or visual acceptance, use actual rendered screenshots. For deployments or external systems, verify those states separately from local code.

If CI or a test result belongs to an older head, do not treat it as proof for the current head.

## 5. Report Findings

Lead with actionable findings ordered by impact. Each finding should contain:

- concise title and severity;
- concrete failure or risk;
- tight file/line or artifact reference;
- violated issue requirement;
- recommended direction, without implementing it in review-only mode.

Then provide the acceptance matrix and a verdict:

- **Ready for PR** — local delivery is verified and no PR exists yet.
- **Ready for merge** — reviewed PR head satisfies the issue and required checks are current.
- **Needs fixes** — actionable implementation defects remain.
- **Needs product decision** — implementation cannot be judged without a missing requirement.
- **Blocked / unavailable evidence** — required proof cannot currently be obtained.

No findings does not automatically mean Ready for merge; required tests, CI, approvals, and deployment gates may still be missing.

## 6. PR and Closure

When PR creation is requested and authorized, ensure the body includes `Closes #<issue>` and the verified evidence. Re-read the created PR and confirm its base/head refs.

Merging is a separate action. Before an authorized merge, verify that the reviewed head has not changed and required checks are current. After merge, verify the PR state and issue closure; do not infer closure from the merge command alone.

Deployment, release, and production verification remain separate workflows unless the issue explicitly includes them and the user authorizes them.
