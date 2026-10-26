---
name: implement-ticket
description: "Orchestrate ticket implementation with TDD, committing, and stacked PR creation."
disable-model-invocation: true
---

# Implement Ticket

Multi-agent orchestration to implement a ticket from ingestion to a published pull request. The primary agent manages the lifecycle, dispatches an implementer subagent for TDD execution to keep contexts focused, and handles git commits, pushes, and PR creation.

## Architecture & Subagent Roles

- **Orchestrator (Primary Agent)**: Manages the lifecycle, inspects tickets, determines the branch base, coordinates the implementer subagent via sparse context pointers, and handles git commits, pushes, and PR creation.
- **Implementer Subagent**: Builds the vertical slice using test-driven development (`tdd` skill), continuously running tests and type checks.

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
   - Determine the public testing seams autonomously, selecting the cleanest testing boundary without interrupting the user.
   - Develop in vertical slices: write a red failing test first, write minimal implementation to turn it green, then verify.
2. **Continuous verification**:
   - Run type checks and targeted tests during each slice cycle.
   - Run the full test suite once implementation finishes.
3. **Completion criterion**:
   - All ticket acceptance criteria satisfied.
   - Full test suite and type checks pass with zero failures.
   - The subagent returns the list of modified files and passing test output to the orchestrator.

### 3. Commit and Push

1. Inspect the working tree to ensure only intended changes are staged.
2. Commit changes with a clear message citing the ticket (e.g. `feat(scope): implement slice (#123)`).
3. Push the feature branch to `origin`.

### 4. Open PR with 'pr' Skill and Manage Stacks

1. **Draft PR body**:
   - Use the `pr` skill template:
     - **Summary**: Concise visual (pseudocode, component tree, call tree, or diff sketch).
     - **Evidence**: Before and after test run outputs.
     - **Merge Danger**: Door type (one-way or two-way) and blast radius assessment.
   - Link the ticket/issue using a GitHub closing keyword (e.g. `Closes #<ticket-id>` or `Closes <ticket-url>`) so merging the PR automatically closes the issue.
2. **Publish PR**:
   - **Standalone PR**:
     - Run `gh pr create --base <base-ref> --title "<title>" --body "<body>"`.
   - **Stacked PR**:
     - If using the `gh stack` extension: run `gh stack submit` to publish or update the PR chain.
     - Otherwise, set the PR base branch explicitly to the blocker branch (`gh pr create --base <blocker-branch> --title "<title>" --body "<body>"`).
     - Note the stack dependency in the PR description.
3. **Completion criterion**:
   - PR is published with the ticket/issue linked via closing keywords (`Closes #<ticket-id>` or `Closes <ticket-url>`).
   - The created PR URL and stack status are returned to the user.
