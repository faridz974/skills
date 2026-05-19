---
name: pr-description
description: Writes concise, copy-paste-ready pull request/merge request descriptions from git diffs, branch comparisons, or summarized code changes. Use when the user asks to write, rewrite, or summarize a Pull Request description, release-facing change summary, or pull request body.
---

# PR Description

## Goal

Produce a Pull Request/Merge Request description that is brief, accurate, and useful to reviewers. Focus on why the change exists and what behavior, architecture, or operational surface changed. Avoid implementation minutiae unless it affects review, deployment, compatibility, or users.

## Quick Start

When the user asks for a Pull Request description:

1. Inspect the git diff against the requested base, usually `main`.
2. Group the change by reviewer-relevant outcomes.
3. Draft a neutral markdown Pull Request body with only the sections that add signal.
4. Call out breaking changes explicitly. If there are none, say `None`.

## What to Include

Always include:

- `Overview`: one short paragraph summarizing the purpose and outcome.
- `Technical Details`: compact metadata such as impacted paths, file count, breaking changes, and related issue.

Include only when relevant:

- `High-Level Architectural Changes`: new boundaries, patterns, data flow, or major design decisions.
- `Key Features`: user-visible or API-visible behavior added, changed, or removed.
- `Performance & Operational Improvements`: reliability, scaling, deployment, monitoring, or runtime impact.
- `Service Changes`: behavior changes grouped by service, package, app, or component.
- `Business Impact`: user, operator, developer, compliance, or product impact.

## Writing Rules

- If the user asks for a complete Pull Request draft, start with a short Pull Request title followed by the body. Otherwise, write only the Pull Request body.
- Prefer outcomes over code-level detail.
- Keep sections concise; omit sections that would only say `None` unless they are required metadata.
- Mention breaking changes clearly. In `Technical Details`, write `Breaking changes: None` when none are apparent.
- If the related issue is unknown, write `Related issue: Not provided`.
- Keep the tone neutral and professional.
- Do not include tool logs, private reasoning, review commentary, or raw implementation notes.

## Suggested Workflow

1. Scan the diff against `main` or the provided base commit.
2. Group changes by architecture, feature behavior, operational impact, and service boundaries.
3. Call out any contract changes, auth/routing changes, or behavior shifts that users would notice.
4. End with a compact technical summary that includes file count, breaking changes, and related issue.

## Example

```md
## Overview

Adds configuration validation for worker startup so invalid queue settings fail before jobs are accepted.

## Key Features

- Validates required queue names and retry limits during service boot.
- Returns clear startup errors for missing or malformed configuration.

## Technical Details

- Impacted paths: `packages/worker/src/config`
- Breaking changes: None
```
