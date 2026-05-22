---
name: review-slop
description: Review a branch or diff for AI-style code artifacts and mismatches with the surrounding codebase. Use when the user asks to check for slop, review a diff for generated-feeling code, audit changes, or identify unnecessary comments, defensive checks, type-escape casts, or inconsistent local style.
---

# Review Slop

Review a diff or branch for code that feels generated, over-abstracted, or inconsistent with nearby code.

Focus on code and tests first. Ignore docs, plans, lockfiles, and generated files unless the user explicitly includes them.

## Scope

This skill is review-only.

- Do not edit files.
- Do not apply patches.
- Do not run full repo or package checks unless the user asks.
- Return findings only.

## Intent Guardrail

Match the user's request exactly.

- Never turn a review request into an implementation task.
- If the user asks for code changes, explain that this skill only performs review and return findings instead.
- Compare the branch against the requested base branch or commit before making findings.
- If no base is specified, use the repository default branch when it is obvious from local context.
- If confidence is low, report an observation instead of a finding.

## Workflow

1. List changed files against the requested base branch or commit.
2. Read the nearest local patterns. Match the file's existing style instead of applying generic cleanup rules.
3. Report only clear branch-introduced slop.
4. Include file and line references.
5. If no clear slop is found, say so explicitly.

## Standard For Calling Something Slop

Only call something slop when there is concrete evidence that it is worse than the surrounding code.

Strong signals:

- explanatory comments that restate obvious code or read like generated prose
- unnecessary try/catch blocks, guard clauses, or defensive branches that do not match trusted call paths in that area
- casts used only to silence type errors, especially `as any`, `as never`, and double-casts
- abstractions added only for indirection without reuse or readability gain
- test expectations for behavior that the codebase does not actually implement
- naming, logging, and control flow that clearly clash with adjacent files

Weak signals:

- a refactor you would not personally choose
- a different but reasonable naming choice
- splitting one function into two small helpers
- extracting shared setup for readability
- introducing a named error for real control flow
- adding small constants for meaningful exit codes

Weak signals are not findings by themselves.

## Review Heuristics

Prefer deleting code over replacing it with a different verbose version.

Treat these as especially suspicious:

- comment blocks above every index, schema field, or obvious branch
- runtime wrappers that exist only to make tests easier but complicate production code
- custom types introduced only to paper over mocks or casts
- branch-only constants, cases, or tests that reference features not present elsewhere in the diff
- defensive logging around normal success paths when adjacent files stay quiet
- test-driven structure that complicates production code without a concrete benefit
- Defensive checks or try/catch blocks that are abnormal for trusted code paths.
- Deeply nested code that should be simplified with early returns.
- Other patterns inconsistent with the file and surrounding codebase.

Keep these when justified by the local area:

- validation at HTTP, queue, webhook, or database boundaries
- lock handling, retries, and error paths around real concurrent or external operations
- test seams that already exist elsewhere in the package
- helper extractions that separate distinct runtime modes or workflows
- small refactors that improve naming or isolate shared setup without changing behavior

## Output

- Findings first, ordered by severity.
- Each finding must include the concrete downside.
- Separate low-confidence observations from clear findings.
- If nothing is clearly wrong, say `no clear slop found`.
