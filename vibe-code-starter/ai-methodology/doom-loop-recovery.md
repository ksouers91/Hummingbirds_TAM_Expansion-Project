# Doom Loop Recovery Protocol

## Purpose

Use this when repeated AI-generated fixes fail, introduce new bugs, or create escalating complexity.

The goal is to stop random patching and return to root-cause analysis.

## Doom loop definition

A doom loop may be happening if:

- The same bug persists after two fixes
- Each fix creates a new bug
- The agent keeps changing broader areas of code
- The explanation becomes vague
- The architecture starts shifting during debugging
- The model says “fixed” but behavior does not change
- The conversation is long and stale
- The bug spans multiple layers

## Immediate rule

After two failed fixes:

STOP CODING.

Do not make another change until root cause is identified.

## Recovery process

### Step 1 — Restate the expected behavior

Answer:

- What should happen?
- What is actually happening?
- How do we reproduce it?
- What changed recently?

### Step 2 — Identify the failing layer

Classify the likely failure area:

- Data/input
- Validation
- State management
- Business logic
- API/controller
- UI/view
- External service
- Environment/config
- Build/dependency
- Test setup

### Step 3 — Trace the flow

Map the path:

```text
Input → validation → processing → state/data update → output/UI/API response
```

Identify where expected and actual behavior first diverge.

### Step 4 — Check for layer mismatch

Common AI bug pattern:

- Data model changed but UI did not
- API changed but caller did not
- Validation changed but tests did not
- State changed but derived state did not
- File path changed but import did not
- Environment variable changed but config did not

### Step 5 — Reduce the problem

Create the smallest reproduction:

- Minimal input
- Minimal code path
- One failing behavior
- No unrelated refactor

### Step 6 — Propose one targeted fix

Before editing, explain:

- Root cause hypothesis
- File(s) to change
- Why these files
- What not to touch
- How to verify fix

### Step 7 — Verify

After fix:

- Run the narrowest relevant test/check
- Confirm the original failure is resolved
- Confirm no obvious adjacent behavior broke
- Summarize result

## Fresh context reset

If the conversation has become stale or tangled, create a reset summary:

```md
# Fresh Context Summary

## Goal
...

## Current architecture
...

## Current bug
...

## Expected behavior
...

## Actual behavior
...

## Relevant files
...

## Failed fixes already tried
...

## Current best root-cause hypothesis
...

## Recommended next step
...
```

Then start a new Claude Code thread/session with that summary.

## Rollback rules

Recommend rollback when:

- A fix touched many unrelated files
- Working behavior regressed
- Architecture changed without approval
- The bug is less severe than the attempted fix
- The model cannot explain why the fix works

## What not to do

Do not:

- Keep applying random patches
- Rewrite large parts of the codebase
- Change architecture during debugging unless approved
- Add dependencies to escape understanding the bug
- Suppress errors without resolving the cause
- Declare success without verification
