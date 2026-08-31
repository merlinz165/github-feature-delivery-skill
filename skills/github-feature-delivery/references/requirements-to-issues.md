# Requirements to GitHub Issues

Use this mode to turn a product requirement, discussion, PRD, design artifact, research result, or backlog idea into implementation-ready GitHub work.

## 1. Establish the Source of Truth

Resolve:

- target repository and default branch;
- relevant repository instructions and product documents;
- existing implementation and tests;
- open Issues and PRs that may duplicate or overlap the request;
- which statements are approved decisions versus proposals or dated research.

Do not claim that the agent remembers all project discussions. Recover useful context, then place the confirmed decisions in repository documents and GitHub issues.

## 2. Extract the Requirement Packet

Capture:

- desired user or system outcome;
- affected users and flows;
- current verified behavior;
- functional requirements;
- data, security, privacy, regional, or reliability constraints;
- success evidence and acceptance criteria;
- dependencies and sequencing;
- explicit non-goals;
- unresolved decisions and assumptions;
- source documents, code paths, designs, and research dates.

Ask a question only when the answer would materially change the architecture, public behavior, safety, data model, destructive effect, or acceptance criteria. Otherwise proceed with a visible, reversible assumption.

## 3. Decide the Issue Topology

Use an Epic when the outcome needs multiple independently deliverable work units. Use a standalone child issue when one pull request can deliver and verify the outcome.

Create separate issue types when useful:

- **Decision** — a human choice must be made before implementation.
- **Design** — a visual or interaction direction must be selected and verified.
- **Foundation** — existing state must be stabilized before parallel feature work.
- **Implementation** — an approved behavior can be delivered through code.
- **Research/Spike** — evidence is needed before a build decision.
- **QA/Verification** — acceptance needs independent runtime or visual proof.

Split work only when each part has its own deliverable value, acceptance evidence, and safe merge boundary. Keep tightly coupled schema/API/UI changes together when separating them would create unusable intermediate states.

Identify:

- **Hard dependency:** the later issue needs the earlier issue's code, data, or decision.
- **Ordering-only constraint:** the issues are logically independent but should not overlap because they touch the same files, environment, or release surface.

## 4. Draft Before Mutating GitHub

Present the proposed Epic, child titles, topology, dependencies, and priority before filing when the user has not already approved the exact issue set.

Do not create placeholder issues. An independent agent session should be able to read the issue and its referenced repository artifacts without relying on the original conversation.

### Epic body template

```markdown
## Outcome
<The product or system outcome, not a list of implementation steps.>

## Why now
<Evidence, user need, or dependency that makes the Epic relevant.>

## Scope
- <Included capability>

## Delivery map
- #<child> — <role in the outcome>

## Dependencies and ordering
- <Hard dependency or ordering-only constraint>

## Success criteria
- <Observable Epic-level result>

## Non-goals
- <Explicitly excluded work>

## Sources
- `<repo path or approved external source>`
```

### Child issue body template

```markdown
## Goal
<One independently deliverable outcome.>

## Documented requirements
<Embed the relevant requirement content. Do not provide only a path.>

## Current verified state
- <Verified code, test, runtime, or data observation>
- <Unknown or unavailable evidence, labeled honestly>

## Scope
1. <Required change>

## Acceptance scenarios
1. <Given/when/then behavior or named observable check>

## Dependencies
- <Hard dependency, ordering-only predecessor, or none>

## Verification
- `<test or inspection command/result expected>`

## Non-goals
- <Excluded work>

## Sources
- `<document or code path>`
```

Use the repository's established issue template when it is stronger; preserve the information above rather than forcing these headings.

## 5. File and Link with Authorization

After authorization:

1. Check duplicates again immediately before creation.
2. Create or reuse labels only when their meaning is clear.
3. Create the Epic and complete child issues.
4. Establish native parent/sub-issue relationships when supported.
5. Verify every issue body, label, link, and relationship by reading it back.
6. Report URLs, topology, and any GitHub capability that was unavailable.

Do not assume that mentioning a child in Markdown created a native sub-issue relationship.

## Completion Result

Return:

- created or updated artifacts;
- dependency/order summary;
- decision gates that still block implementation;
- recommended first delivery issue;
- verification that remote GitHub state matches the approved plan.
