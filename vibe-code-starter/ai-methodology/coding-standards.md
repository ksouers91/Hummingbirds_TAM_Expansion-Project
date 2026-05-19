# Coding Standards

## Purpose

These standards keep AI-generated code maintainable, understandable, and safe to iterate.

## General principles

- Prefer clarity over cleverness.
- Prefer boring, proven patterns over novel abstractions.
- Keep files and functions focused.
- Avoid broad rewrites unless required.
- Avoid adding dependencies without a clear reason.
- Make the smallest useful change.
- Preserve existing behavior unless a change is explicitly approved.

## Code organization

Use simple, predictable structure.

Recommended patterns:

- Separate UI, business logic, data access, and integration code when applicable.
- Keep configuration in one obvious place.
- Keep reusable helpers small and named clearly.
- Avoid circular dependencies.
- Avoid dumping unrelated logic into one file.

## Naming

Names should explain purpose.

Good:
- `validateCsvRows`
- `fetchCustomerById`
- `formatCurrency`
- `buildExportFile`

Avoid:
- `handleStuff`
- `processData`
- `doThing`
- `utils2`

## Comments

Use comments to explain why, not obvious what.

Good:
```js
// Use a stable sort so rows with equal scores preserve import order.
```

Avoid:
```js
// Loop through rows.
```

## Dependencies

Before adding a dependency, explain:

1. What problem it solves
2. Why native/platform functionality is not enough
3. Maintenance/security tradeoffs
4. Whether it is necessary for V1

Do not add a dependency only because it is convenient.

## Error handling

Handle errors where they can be resolved or explained.

Good error handling:
- Tells the user what happened
- Gives a next step when possible
- Avoids exposing sensitive internals
- Logs useful technical detail when appropriate

Avoid:
- Silent failures
- Catch-all blocks that hide bugs
- Unclear messages like “Something went wrong”

## Input validation

Validate all external input:

- User input
- File contents
- API responses
- Environment variables
- Database records
- URL/query parameters

Assume external data may be missing, malformed, duplicated, or hostile.

## State management

Keep state as simple as possible.

Before adding complex state management, confirm:
- What state exists?
- Who owns it?
- Where does it change?
- What can get out of sync?
- How do we reset or recover?

## Testing philosophy

Testing should match project risk.

For prototypes:
- Test critical flows manually
- Add lightweight automated tests for tricky logic

For internal tools:
- Add tests for core business logic and edge cases

For production:
- Add tests for core flows, failure modes, and regressions

## Security basics

- Never commit secrets.
- Use environment variables for credentials.
- Validate inputs.
- Avoid custom auth/security logic when proven libraries exist.
- Do not log sensitive data unless explicitly justified.
- Treat payments, auth, medical, financial, and personal data as high risk.

## Performance basics

- Do not optimize prematurely.
- Do avoid obviously wasteful logic.
- Measure before optimizing when performance matters.
- Consider expected input size before choosing an approach.

## Documentation

When meaningful changes are made, update:

- `PROJECT_BRIEF.md` for scope/requirements changes
- `BUILD_PLAN.md` for phase/task changes
- `DECISIONS.md` for meaningful architecture/dependency decisions
- `README.md` for setup/run instructions

## Completion standard

A task is not complete until Claude can explain:

1. What changed
2. Why it was implemented this way
3. How to run or verify it
4. What risks remain
5. What was intentionally not done
