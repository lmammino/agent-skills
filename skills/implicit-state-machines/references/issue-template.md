# Issue template

Match the repository's own issue format first: its title prefix (for
example a priority), its sections and its labels. Use this template for
the content. Ask for any label or priority that the user did not name.

## Main issue

```markdown
<tracking line, if the repository uses one>

**Area:** <area>
**Priority:** <priority>

## Summary
<what an implicit state machine is, in one sentence>
<counts, and the main points as bullets>

## Bugs
<bug table from the report: ID, what happens, where, confidence>
<shared causes>
<how to fix each one test-first>

## Explicit states with no behavior change
<Now table: ID, state machine, today, proposal>
<edge cases to keep>

## Follow-up work outside this issue
<Issue table: ID, state machine, reason to defer, target issue>

## No change
<Leave list and the already-explicit holders>

## Test access hazard
<hazards, with counts, and the items they affect>

## Plan
<the PRs in order, with titles>

## Acceptance criteria
- [ ] <one checkable item for each bug, each Now group and each record step>
- [ ] <the project checks to run, and the device runs>

## Decisions (<date>)
<numbered decisions, each with its date>

## Product rules to keep
<the project rules that the changes must keep>

## Dependencies / related work
<linked issues>
```

## Follow-up issue

```markdown
<tracking line>
Follows: #<main issue>

**Area:** <area>
**Priority:** <priority>

## Context
<the state machine, its fields with file:line, its writers and readers>
<the callers and tests that a change touches>

## Proposal
<the target type and the changes>

## Product rules to keep
<only the rules this change touches>

## Acceptance criteria
- [ ] <checkable items, the checks to run, the device runs>

## Dependencies / related work
<the main issue, and the work that must land first>
```

## Umbrella comment

When an existing issue covers a group of findings, comment there with a
table (ID, state machine, today, proposal), a suggested order, the
hazards that affect it, and links to the main issue and the follow-up
issues.
