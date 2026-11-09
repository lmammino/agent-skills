# Report template

Use stable IDs: `S1`, `S2`, ... for state machines and `B1`, `B2`, ...
for bugs, in scan order. Link each bug to its state machine, for example
"B1 (S4)". Keep the IDs in later turns, issues and PRs.

Give `file:line` for every location.

## 1. Summary

- Explain an implicit state machine in one sentence.
- Give the counts: state machines, bugs, Now, Issue, Leave.
- Give the main points in 3 to 5 bullets.

## 2. Scope

What you scanned, the candidate file count, and what you skipped and
why.

## 3. Hazards

Each hazard, with counts and files, and the Now items it affects.

## 4. Bugs

| ID | What happens (as the user sees it) | Where | Confidence | Regression test |
|---|---|---|---|---|

Then:

- the shared causes, when two or more bugs have one cause;
- the rejected candidates, one line each.

## 5. Now

| ID | State machine and its states | Today | Proposal | State mapping | Risk |
|---|---|---|---|---|---|

Then the edge cases to keep, one line each.

## 6. Issue

| ID | State machine | Reason to defer | Target issue |
|---|---|---|---|

## 7. Leave

One line for each item: the ID and the reason. Then the already-explicit
state holders in one line.

## 8. Plan

The PRs in phase 5 order, with the checks for each. Name the checks that
cannot run now, and the reason.

## 9. Questions

Numbered decisions for the user. Give a recommendation for each one.
