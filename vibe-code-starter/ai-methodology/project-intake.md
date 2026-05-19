# Project Intake Methodology

## Purpose

Use this file before coding starts. The goal is to collect enough context to build the right thing without creating an enterprise-length questionnaire.

Ask only questions that materially affect implementation, architecture, risk, or scope.

## Intake rules for Claude

1. Start with the core questions.
2. Ask follow-ups only when ambiguity creates meaningful risk.
3. Do not ask questions that can be reasonably deferred.
4. After intake, summarize answers into `PROJECT_BRIEF.md`.
5. Identify the project type and relevant methodology modules.
6. Ask for approval before coding.

## Core intake questions

Ask these first:

1. What are we building in one sentence?
2. Who will use this?
3. What should the user be able to do in V1?
4. Is this a prototype, internal tool, automation, learning project, or production app?
5. What tech stack should we use, if already known?
6. Where will this run? Local machine, browser, server, GitHub Pages, Vercel, etc.
7. What data does this use or create?
8. Are there external integrations, APIs, files, databases, or auth requirements?
9. What are the most important edge cases or failure modes?
10. What should be explicitly out of scope for V1?
11. What does “done” mean for this project?
12. Are there any security, privacy, or production concerns?

## Project classification

Classify the project after intake.

### Prototype

Use when:
- Goal is learning, validation, or quick demo
- Speed matters more than polish
- Production risk is low

Required modules:
- coding-standards.md
- reviewer-agent.md
- doom-loop-recovery.md

Usually skip:
- production-readiness.md unless deployment is needed

### Internal tool

Use when:
- A small group will use it
- Correctness and usability matter
- Production-grade scale may not matter

Required modules:
- coding-standards.md
- architecture-principles.md
- reviewer-agent.md
- doom-loop-recovery.md

Use if deployed:
- production-readiness.md

### Small automation

Use when:
- Script automates repetitive work
- Inputs/outputs are files, APIs, or reports
- Reliability matters more than UI

Required modules:
- coding-standards.md
- reviewer-agent.md
- doom-loop-recovery.md

Special focus:
- input validation
- dry-run mode
- backup/rollback
- logging

### Production application

Use when:
- Real users depend on the app
- Data loss, downtime, or security matter

Required modules:
- coding-standards.md
- architecture-principles.md
- reviewer-agent.md
- doom-loop-recovery.md
- production-readiness.md

Special focus:
- testing
- secrets
- auth
- logging
- deployment
- rollback

### Security-sensitive application

Use when:
- Auth, payments, medical, financial, personal, or confidential data is involved

Required behavior:
- Do not vibe code blindly
- Require human expert review
- Document threat assumptions
- Prefer proven libraries and frameworks
- Avoid custom security logic unless absolutely necessary

### Performance-sensitive application

Use when:
- Latency, scale, large data, or high throughput matters

Required behavior:
- Define performance expectations
- Avoid premature optimization
- Add measurement before optimization
- Explain performance tradeoffs

### Integration-heavy application

Use when:
- Multiple APIs/services are involved
- Failures may happen outside the codebase

Required behavior:
- Define integration contracts
- Handle retries/failures
- Log integration errors
- Avoid silent failures

## Output after intake

Claude should produce:

1. Completed or updated `PROJECT_BRIEF.md`
2. Proposed `BUILD_PLAN.md`
3. Initial risk list
4. Recommended methodology modules
5. Questions that remain unresolved
6. Clear ask for user approval before coding

## Intake completion prompt

After gathering information, Claude should say:

```text
I have enough to draft the initial project brief and build plan. I’ll summarize the project, classify the project type, identify the relevant methodology modules, and propose the first implementation checkpoint. Please review before I write code.
```
