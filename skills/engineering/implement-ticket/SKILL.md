---
name: implement-ticket
description: "Orchestrate ticket implementation across subagents with TDD, two-round review and fixing, and stacked PR creation."
disable-model-invocation: true
---

# Implement Ticket

Multi-agent orchestration to implement a ticket from ingestion to a published pull request. The primary agent orchestrates specialized subagents for implementation, review, and fixing to keep contexts focused and enforce clean verification boundaries.

## Architecture & Subagent Roles

- **Orchestrator (Primary Agent)**: Manages the lifecycle, inspects tickets, determines the branch base, coordinates subagents via sparse context pointers, and handles git commits, pushes, and PR creation.
- **Implementer Subagent**: Builds the vertical slice using test-driven development (`tdd` skill), continuously running tests and type checks.
- **Review Subagents**: Execute independent reviews (`code-review` for standards and spec alignment; `thermo-nuclear-code-quality-review` for structural simplification and maintainability).
- **Fixer Subagent**: Resolves review findings and verifies regression-free passes before control returns to the orchestrator.

## Workflow

### 1. Ingest Ticket and Plan Branching

1. **Ingest ticket reference**:
   - Check for a user-supplied ticket reference (URL, issue key, number, or local file under `.scratch/` or `issues/`). If none was provided, ask the user before doing anything else.
   - Fetch the ticket from the issue tracker or local path.
   - Extract and state the ticket title, acceptance criteria, scope boundaries, and declared dependencies (blockers).

2. **Determine base and branch strategy**:
   - Inspect dependencies: Does this ticket depend on an unmerged ticket or upstream work?
   - **Standalone branch**: If there are no active blockers, base the feature branch off the latest tracking branch (e.g. `main`).
   - **Stacked branch**: If the ticket depends on an unmerged blocker ticket whose branch is active, branch off that blocker's feature branch.
   - Create the feature branch:
     - When using `gh stack`: run `gh stack new <branch-name>` or create the branch off the blocker.
     - When using standard git: checkout the base ref and create `feat/<ticket-id>-<slug>`.
   - Record `<base-ref>` (either `main` or `<blocker-branch>`) for downstream diffing and PR targeting.

### 2. Implement with TDD Subagent

Dispatch an **implementer subagent** with the ticket details, target branch, and `<base-ref>`:

1. **Invoke TDD skill**:
   - The subagent calls the `tdd` skill.
   - Confirm public testing seams with the user before writing tests.
   - Develop in vertical slices: write a red failing test first, write minimal implementation to turn it green, then verify.
2. **Continuous verification**:
   - Run type checks and targeted tests during each slice cycle.
   - Run the full test suite once implementation finishes.
3. **Completion criterion**:
   - All ticket acceptance criteria satisfied.
   - Full test suite and type checks pass with zero failures.
   - The subagent returns the list of modified files and passing test output to the orchestrator.

### 3. Review Round 1: Code Review & Fix

Run a two-axis review against the base reference (`<base-ref>...HEAD`):

1. **Dispatch review**:
   - Run the `code-review` skill across two parallel sub-agents:
     - **Standards axis**: Checks adherence to repo coding standards and the Fowler smell baseline.
     - **Spec axis**: Checks diff against the ticket's acceptance criteria for missing logic or scope creep.
   - Aggregate findings under `## Standards` and `## Spec`.
2. **Fix findings**:
   - If major or blocking findings are identified, dispatch a **fixer subagent** with the report and diff.
   - The fixer subagent resolves all findings, then runs type checks and the full test suite.
3. **Completion criterion**:
   - Zero unresolved major standards violations or spec discrepancies.
   - Test suite and type checks remain green.

### 4. Review Round 2: Thermo-Nuclear Quality Review & Fix

Subject the verified code to a stricter architectural and maintainability audit:

1. **Dispatch review**:
   - Run the `thermo-nuclear-code-quality-review` skill against `<base-ref>...HEAD`.
   - Reviewer audits for:
     - Structural simplification and "code judo" opportunities that delete incidental complexity.
     - File size discipline (no files pushed over 1,000 lines without justification).
     - Elimination of ad-hoc branching, special-case conditionals, or leaky abstractions.
     - Clean type contracts and reuse of canonical helpers.
2. **Fix findings**:
   - If structural regressions or clear simplification opportunities are flagged, dispatch a **fixer subagent**.
   - The fixer subagent implements the structural improvements while preserving external behavior.
   - Re-run type checks and the full test suite.
3. **Completion criterion**:
   - Meets the thermo-nuclear approval bar (no structural regressions, no spaghetti growth, clean boundaries).
   - Test suite and type checks remain green.

### 5. Commit and Push

1. Inspect the working tree to ensure only intended changes are staged.
2. Commit changes with a clear message citing the ticket (e.g. `feat(scope): implement slice (#123)`).
3. Push the feature branch to `origin`.

### 6. Open PR with 'pr' Skill and Manage Stacks

1. **Draft PR body**:
   - Use the `pr` skill template:
     - **Summary**: Concise visual (pseudocode, component tree, call tree, or diff sketch).
     - **Evidence**: Before and after test run outputs.
     - **Merge Danger**: Door type (one-way or two-way) and blast radius assessment.
2. **Publish PR**:
   - **Standalone PR**:
     - Run `gh pr create --base <base-ref> --title "<title>" --body "<body>"`.
     - Link or close the ticket reference.
   - **Stacked PR**:
     - If using the `gh stack` extension: run `gh stack submit` to publish or update the PR chain.
     - Otherwise, set the PR base branch explicitly to the blocker branch (`gh pr create --base <blocker-branch> --title "<title>" --body "<body>"`).
     - Note the stack dependency in the PR description.
3. Return the created PR URL and stack status to the user.
