# Build Plan

## Build strategy

Use small, reviewable phases. Do not jump into full implementation until the brief and architecture are approved.

## Current phase

Phase 0 — Intake and planning

## Phase 0 — Intake and planning

### Goal

Clarify the project enough to avoid avoidable rework.

### Tasks

- [ ] Complete `PROJECT_BRIEF.md`
- [ ] Confirm project type
- [ ] Confirm tech stack
- [ ] Confirm V1 scope
- [ ] Identify risks and edge cases
- [ ] Confirm what is out of scope

### Exit criteria

- [ ] Project goal is clear
- [ ] Users are defined
- [ ] V1 requirements are defined
- [ ] Major risks are known
- [ ] User approves moving to architecture

---

## Phase 1 — Architecture

### Goal

Define the simplest architecture that can support V1 without over-engineering.

### Tasks

- [ ] Propose folder structure
- [ ] Identify major modules/components
- [ ] Define data flow
- [ ] Define state management approach if needed
- [ ] Define integration boundaries
- [ ] Identify security-sensitive areas
- [ ] Record decisions in `DECISIONS.md`

### Exit criteria

- [ ] Architecture is explained in plain English
- [ ] Tradeoffs are documented
- [ ] User approves implementation direction

---

## Phase 2 — Scaffold

### Goal

Create the minimum project skeleton required to begin implementation.

### Tasks

- [ ] Initialize project structure
- [ ] Add basic configuration
- [ ] Add linting/formatting if appropriate
- [ ] Add minimal README instructions
- [ ] Confirm project runs locally

### Exit criteria

- [ ] Project starts successfully
- [ ] Folder structure matches approved architecture
- [ ] No unnecessary dependencies added

---

## Phase 3 — Core functionality

### Goal

Build the smallest working version of the core workflow.

### Tasks

- [ ] Implement primary user flow
- [ ] Add input validation
- [ ] Add basic error handling
- [ ] Add simple tests where useful
- [ ] Confirm expected outputs

### Exit criteria

- [ ] Core workflow works end-to-end
- [ ] Known edge cases are handled or documented
- [ ] Implementation is reviewed

---

## Phase 4 — Review and hardening

### Goal

Improve quality without unnecessary expansion.

### Tasks

- [ ] Run reviewer-agent checklist
- [ ] Run edge-case review
- [ ] Add missing tests
- [ ] Improve errors and empty states
- [ ] Remove unused code
- [ ] Update documentation

### Exit criteria

- [ ] Review findings addressed or documented
- [ ] No obvious security issues
- [ ] No known broken critical flows

---

## Phase 5 — Release/deployment readiness

### Goal

Prepare for actual use.

### Tasks

- [ ] Confirm environment variables
- [ ] Confirm secrets are not committed
- [ ] Confirm deployment instructions
- [ ] Confirm rollback approach
- [ ] Confirm logging/monitoring needs
- [ ] Update `REVIEW_CHECKLIST.md`

### Exit criteria

- [ ] Project can be run or deployed by following documented steps
- [ ] Known limitations are documented
- [ ] User approves release/use

---

## Active task list

- [ ] TBD

## Blockers

- TBD

## Next checkpoint

TBD
