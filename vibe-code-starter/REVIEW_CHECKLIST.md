# Review Checklist

Use this before calling a feature or project complete.

## Scope review

- [ ] The implementation matches `PROJECT_BRIEF.md`
- [ ] No major out-of-scope work was added without approval
- [ ] Any shortcuts are documented
- [ ] Any assumptions are documented

## Code quality

- [ ] Code is readable
- [ ] Code is modular enough for the project size
- [ ] Naming is clear
- [ ] Duplicate logic is minimized
- [ ] No unused files, functions, variables, or dependencies remain
- [ ] Comments explain why, not obvious what

## Architecture

- [ ] Data flow is understandable
- [ ] Responsibilities are separated clearly
- [ ] No unnecessary abstraction was introduced
- [ ] No single file or function is doing too much
- [ ] Architecture decisions are recorded in `DECISIONS.md`

## Testing

- [ ] Core happy path is tested manually or automatically
- [ ] Important edge cases are tested
- [ ] Error states are tested
- [ ] Tests are documented if not automated
- [ ] Known untested areas are listed

## Security

- [ ] No secrets are committed
- [ ] Inputs are validated
- [ ] Sensitive data is not logged unnecessarily
- [ ] Permissions/auth assumptions are documented
- [ ] External API usage is reviewed
- [ ] Dependencies are reasonable and necessary

## Reliability

- [ ] Network failures are handled where relevant
- [ ] Missing/invalid data is handled
- [ ] User-facing errors are understandable
- [ ] System fails safely where possible

## Performance

- [ ] No obvious inefficient loops for expected data size
- [ ] Large input behavior is considered
- [ ] Unnecessary repeated API/database calls are avoided
- [ ] Performance-sensitive areas are documented

## UX / usability

- [ ] Primary user flow is clear
- [ ] Empty states are handled
- [ ] Error states are helpful
- [ ] User does not need hidden knowledge to use the app

## Documentation

- [ ] Setup instructions are current
- [ ] Run instructions are current
- [ ] Environment variables are documented
- [ ] Known limitations are documented
- [ ] Next steps are documented

## Final Claude self-review

Before final response, Claude must answer:

1. What changed?
2. Why was it implemented this way?
3. What could break?
4. What did you intentionally not do?
5. What should be reviewed by a human?
6. What is the next highest-leverage improvement?
