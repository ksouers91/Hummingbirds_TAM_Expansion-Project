# Architecture Principles

## Purpose

Use this before building anything larger than a simple one-file script.

The goal is to create enough structure to prevent chaos without over-engineering.

## Core principles

- Architecture should match project risk and complexity.
- Start simple, but not sloppy.
- Separate responsibilities when it reduces confusion.
- Avoid abstractions that do not yet solve a real problem.
- Make data flow easy to trace.
- Document meaningful tradeoffs.

## Architecture questions

Before implementation, answer:

1. What are the major parts of the system?
2. What owns the data?
3. Where does state live?
4. How does data move through the app?
5. What external systems are involved?
6. What can fail?
7. What needs to be easy to change later?
8. What should stay intentionally simple for V1?

## Recommended architecture output

Claude should propose:

```md
# Architecture Proposal

## Summary
Plain-English overview.

## Major components
- ...

## Data flow
Input → processing → storage/state → output

## Folder structure
...

## Key decisions
- ...

## Tradeoffs
- ...

## Risks
- ...

## What we are intentionally not building yet
- ...
```

## Layer guidance

### Data layer

Responsible for:
- Storage
- Fetching
- Persistence
- Data shape

Avoid:
- UI logic
- Presentation formatting
- Unrelated business rules

### Business logic layer

Responsible for:
- Rules
- Calculations
- Validation
- Transformations

Avoid:
- Direct UI rendering
- Hardcoded external service assumptions

### Controller/API layer

Responsible for:
- Request handling
- Calling business logic
- Returning responses
- Error boundaries

Avoid:
- Too much business logic
- Silent error swallowing

### View/UI layer

Responsible for:
- Display
- User interaction
- Form feedback
- Empty/error states

Avoid:
- Complex business rules
- Direct database access

## Over-engineering warning signs

- Multiple layers before there is meaningful complexity
- Generic abstractions with one use case
- Premature plugin systems
- Complex state management for simple state
- Too many files before core behavior works
- Introducing queues, workers, or microservices too early

## Under-engineering warning signs

- One file handles everything
- Business logic buried in UI
- No validation boundary
- No clear error handling
- Data shape changes in many places
- No documentation for important decisions

## Decision rule

Choose the simplest architecture that:

1. Solves V1
2. Keeps data flow understandable
3. Allows likely next changes
4. Does not create unnecessary maintenance burden
