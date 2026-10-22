---
name: implement-ticket
description: "Implement a ticket on a fresh feature branch, review and fix quality findings, and open a PR."
disable-model-invocation: true
---

Implement the work described in a single ticket from start to PR.

## Workflow

### 1. Identify the ticket

- Check if the user passed a ticket reference (URL, issue key, number, or title).
- If not provided, ask the user for the ticket reference before proceeding.
- Fetch the ticket from the issue tracker. State its key, title, and acceptance criteria to confirm understanding before writing code. If ambiguous, ask.

### 2. Create a feature branch

- Ensure the working tree is clean and the base branch (usually `main`) is up to date.
- Create and check out a dedicated feature branch named after the ticket (e.g. `feat/<ticket-id>-<slug>`).

### 3. Implement the work

- Implement all requirements and edge cases specified in the ticket.
- Call the Skill tool with `tdd` where applicable, developing in vertical slices at pre-agreed seams.
- Run type checks and targeted tests continuously during development, followed by the full test suite once implementation is complete.

### 4. Review and fix findings (pre-commit)

Do not commit yet. Run both review skills against the uncommitted diff:

1. Call the Skill tool with `code-review` to check standards and spec alignment. Fix all major findings.
2. Call the Skill tool with `thermo-nuclear-code-quality-review` to audit maintainability, abstractions, and structural complexity. Fix all major findings.
3. Re-run type checks and the full test suite to verify fixes introduced no regressions.

### 5. Commit and push

- Stage the verified changes.
- Commit to the feature branch with a concise, descriptive commit message referencing the ticket identifier.
- Push the feature branch to `origin`.

### 6. Open a PR

- Open a pull request against the base branch using GitHub CLI (`gh pr create`) or the repository's PR mechanism.
- Mark the PR as closing or referencing the ticket.
- Use the `pr` skill template for the body: concise summary visual/diff, before/after evidence, and merge danger evaluation.
- Return the created PR URL to the user.
