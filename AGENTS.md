# AI Job Application Tracker

## Project Overview

A personal job application tracking system.

The primary goal is to manage job applications,
interviews, and follow-up tasks.

The project is currently in the requirements
and design phase.

## Source of Truth

- Product requirements: docs/PRD.md
- Do not invent requirements.
- Ask for clarification when requirements are ambiguous.
- Do not expand the MVP scope without approval.

## Engineering Principles

- Prefer simple, maintainable solutions.
- Avoid premature optimization.
- Avoid unnecessary abstractions.
- Explain significant technical trade-offs.
- Challenge assumptions when appropriate.

## Development Workflow

Before implementing a feature:

1. Read the relevant requirements.
2. Identify ambiguities and missing information.
3. Propose an implementation plan.
4. Identify risks and edge cases.
5. Wait for approval before implementation.

## Current Restrictions

- Do not implement application code.
- Do not install dependencies.
- Do not modify the PRD without approval.
- Focus on requirements analysis and system design.

## Review Guidelines

When reviewing requirements:

- Identify contradictions.
- Identify missing business rules.
- Identify unclear acceptance criteria.
- Identify unnecessary complexity.
- Rank findings by severity.
- Provide concrete recommendations.

## Git Workflow

After completing any task that modifies repository files:

1. Review the changes using `git diff`.
2. Run relevant validation or tests when applicable.
3. Stage only the files changed for the current task.
4. Create one Git commit with a clear, descriptive message.
5. Push the commit to the current branch's configured remote.

Rules:
- Do not commit unrelated changes.
- Do not commit secrets, credentials, or temporary files.
- Do not create empty commits.
- Do not amend existing commits.
- Never force-push.
- Do not switch branches without explicit approval.
- If there is no remote or upstream branch configured,
  report the issue instead of guessing.
- If validation fails, do not commit or push silently.
- If pushing fails, report the error and preserve the local commit.

After completion, report:
- Commit hash
- Commit message
- Branch name
- Push status
- Validation results
