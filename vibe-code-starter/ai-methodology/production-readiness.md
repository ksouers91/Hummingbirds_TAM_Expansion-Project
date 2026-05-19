# Production Readiness

## Purpose

Use this when a project may be used by real users, handle important data, or be deployed beyond a local prototype.

## Production readiness rule

Do not assume a project is production-ready just because it works locally.

Production readiness requires review across:

- Security
- Reliability
- Testing
- Deployment
- Observability
- Data handling
- Rollback
- Documentation

## Security checklist

- [ ] No secrets are committed
- [ ] Environment variables are documented
- [ ] Inputs are validated
- [ ] Auth is handled by proven tools/patterns
- [ ] Authorization rules are explicit
- [ ] Sensitive data is not logged unnecessarily
- [ ] Dependencies are necessary and reasonably trusted
- [ ] Error messages do not leak sensitive internals

## Reliability checklist

- [ ] Core user flow handles failure states
- [ ] External API failures are handled
- [ ] Network failures are handled where relevant
- [ ] Missing data does not crash the system
- [ ] Duplicate actions are considered
- [ ] Long-running actions have appropriate feedback
- [ ] Data loss risks are identified

## Testing checklist

- [ ] Happy path tested
- [ ] Important edge cases tested
- [ ] Error paths tested
- [ ] Integration behavior tested or documented
- [ ] Manual test plan exists if automated tests are not present
- [ ] Known gaps are documented

## Deployment checklist

- [ ] Build command documented
- [ ] Run command documented
- [ ] Deployment target documented
- [ ] Environment variables documented
- [ ] Database migrations documented if relevant
- [ ] Rollback plan documented
- [ ] Domain/CORS/config assumptions documented if relevant

## Observability checklist

- [ ] Errors are visible somewhere useful
- [ ] Logs contain enough detail to debug
- [ ] Logs avoid sensitive data
- [ ] Important user actions can be traced if needed
- [ ] Monitoring needs are documented if relevant

## Data checklist

- [ ] Data model is documented
- [ ] Data retention assumptions are documented
- [ ] Backup/export needs are considered
- [ ] Destructive actions require confirmation where appropriate
- [ ] Migration risks are identified

## Human review required

Require human expert review for:

- Payment processing
- Authentication/authorization
- Medical, financial, legal, or highly sensitive data
- Production infrastructure
- Network/security configuration
- Database migrations that could lose data
- Compliance requirements
- High-scale performance requirements

## Production readiness output

Claude should produce:

```md
# Production Readiness Summary

## Ready?
Yes / No / Partially

## Blocking issues
- ...

## Non-blocking risks
- ...

## Required human review
- ...

## Deployment notes
- ...

## Rollback plan
- ...

## Known limitations
- ...
```
