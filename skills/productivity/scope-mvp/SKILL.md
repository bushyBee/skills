---
name: scope-mvp
description: Cut an idea down to the smallest working MVP that tests its core promise, with must-have parts and clear acceptance criteria.
disable-model-invocation: true
---

Scope a product, service, feature within an existing offering, or business idea into one **thin slice**: a working journey that tests the most important promise. The deliverable is a scope brief in this conversation, not an implementation or a full product roadmap.

## Process

### 1. Choose the core promise

Use the conversation to identify one target user, their biggest pain point, and the single useful outcome this MVP must deliver. When several outcomes compete, recommend the one whose failure would most undermine the idea and explain why.

For an existing product, inspect only the context needed to identify reusable capabilities and integration constraints. Ask questions only when an answer would materially change the core outcome, scope, or test; otherwise state assumptions and proceed. Keep clarification focused on the first experiment rather than designing the whole business.

This step is complete when the user, pain point, and outcome can be stated in one sentence, with any unresolved assumption explicit.

### 2. Define the first proof

Identify the most consequential uncertainty: whether the core capability is feasible or whether it helps the intended user. Choose the experiment that addresses that uncertainty first.

Describe who will test it, the representative input or situation, the observable result, and what counts as pass or fail. Propose a threshold when needed and label it as proposed; distinguish observed evidence from assumptions. A working technical demonstration establishes feasibility, not customer demand.

This step is complete when the result would support a concrete decision to continue, revise, or stop.

### 3. Cut to the thin slice

Trace one journey from input to useful result. Keep the capability under test real. Surrounding work may be manual, reused, or simulated when that shortcut preserves the validity of the experiment; make those boundaries explicit. For a service or business, the journey may be delivered manually rather than through new software.

Apply this **removal test** to every proposed part:

> If this were removed, could the intended user still complete the core journey and could we still evaluate the result safely?

Keep a part only if its removal breaks the journey, invalidates the test, or creates unacceptable risk. Include the dependencies and safeguards necessary for this particular experiment. Preserve explicit user constraints; if they prevent a smaller valid experiment, explain the tradeoff.

Prefer a bounded user group, input type, environment, or operating limit over generalized infrastructure. Judge accounts, payments, automation, dashboards, integrations, and polish by the same removal test as every other feature. Move optional ideas to the sidelines, without designing their implementation or scheduling future releases.

This step is complete when every retained part has a necessity rationale and an observable acceptance criterion, and removing any remaining part would invalidate the experiment.

### 4. Deliver the scope brief

Use the format below. Keep each section brief and proportional to the idea; the must-have list is the primary deliverable. Include only the parts needed for the first proof, not a target number of features or an exhaustive user-story inventory.

## Output format

### Core promise

For [one target user] facing [pain point], deliver [one useful outcome].

### First proof

- **Uncertainty:** The assumption this experiment tests.
- **Test:** Who tries what, using which representative input or situation.
- **Pass/fail:** Observable evidence and the decision it enables; label proposed thresholds.

### Must-have parts

A short numbered list in dependency order. For each part include:

- **Part:** The minimum capability or operational step to deliver.
- **Necessary because:** What breaks or becomes untestable without it.
- **Done when:** An observable acceptance criterion.

### Shortcuts and constraints

State what is manual, reused, or simulated, the operating limits, and any assumptions that could change the scope. Distinguish the real core capability from its supporting shortcuts.

### Sidelines

List excluded ideas with a brief reason for each. Where useful, name the evidence that would justify reconsidering an item rather than promising it as the next release.

### Stop rule

State when building ends and the first test begins. Explain what result warrants continuing, revising, or stopping. If a prerequisite feasibility check fails, revisit the scope before building dependent parts.

End by asking whether this boundary captures the user's intended first proof. If they add a feature, apply the removal test and explain whether it replaces a retained part, is a prerequisite, or belongs on the sidelines. Incorporate corrections without silently widening the experiment.
