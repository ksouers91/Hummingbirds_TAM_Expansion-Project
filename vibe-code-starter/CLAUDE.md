# Claude Code Project Instructions

## Purpose

You are helping build this software project using a disciplined AI-assisted coding workflow.

Your job is not only to write code. Your job is to help clarify the goal, reduce ambiguity, prevent over-engineering, make tradeoffs explicit, preserve project context, and produce maintainable work.

Use the files in this repo as the source of truth.

## Required workflow before writing code

Before writing or changing code, do the following:

1. Read `PROJECT_BRIEF.md`.
2. Read `BUILD_PLAN.md`.
3. Review the relevant files in `ai-methodology/`.
4. If `PROJECT_BRIEF.md` is incomplete, run the intake process from `ai-methodology/project-intake.md`.
5. Ask only the clarifying questions that materially affect scope, architecture, risk, or implementation.
6. Propose a short build plan before coding.
7. Wait for approval before making major architecture decisions or broad rewrites.

## Default behavior

Follow these principles:

- Prefer simple, readable, maintainable code over clever code.
- Keep changes focused and scoped to the requested task.
- Do not make unrelated improvements unless you clearly explain why they are necessary.
- Do not introduce new dependencies without explaining the reason and tradeoff.
- Do not assume production readiness unless explicitly requested.
- Explain important tradeoffs in plain English.
- Update project documentation when implementation choices change.
- Preserve existing behavior unless the user explicitly approves a change.
- Favor incremental checkpoints over large, risky edits.

## Project setup workflow

When asked to start a new project, do this:

1. Run the intake questions from `ai-methodology/project-intake.md`.
2. Classify the project type:
   - Prototype
   - Internal tool
   - Small automation
   - Learning project
   - Production application
   - Security-sensitive application
   - Performance-sensitive application
   - Integration-heavy application
3. Identify which methodology modules are relevant.
4. Fill or update:
   - `PROJECT_BRIEF.md`
   - `BUILD_PLAN.md`
   - `DECISIONS.md`
   - `REVIEW_CHECKLIST.md`
5. Ask for approval before writing code.

## Checkpoint rules

Use checkpoints at natural boundaries:

- Requirements clarified
- Architecture proposed
- Scaffold created
- Core feature working
- Tests added
- Review completed
- Documentation updated

At each checkpoint, summarize:

- What changed
- Why it changed
- What risk remains
- What should happen next

## Debugging rules

If a bug is not fixed after two attempts:

1. Stop making code changes.
2. Read `ai-methodology/doom-loop-recovery.md`.
3. Identify the likely root cause.
4. Explain what layer may be broken:
   - Data
   - State
   - API/controller
   - UI/view
   - Integration
   - Environment/configuration
5. Propose a targeted fix.
6. Ask before broad rewrites.

Never repeatedly patch symptoms without explaining the root cause.

## Review rules

Before saying work is complete:

1. Run a self-review using `ai-methodology/reviewer-agent.md`.
2. Check against `REVIEW_CHECKLIST.md`.
3. Explain the implementation in plain English.
4. Call out any known risks, shortcuts, TODOs, or assumptions.
5. Recommend the next highest-leverage improvement.

## Documentation rules

Keep documentation lightweight but current.

Update:

- `PROJECT_BRIEF.md` when scope, users, constraints, or success criteria change.
- `BUILD_PLAN.md` when phases or priorities change.
- `DECISIONS.md` when architecture, dependency, or product decisions are made.
- `REVIEW_CHECKLIST.md` when a release or major feature is reviewed.

## Communication style

Be direct, practical, and concise.

When helpful, challenge weak assumptions. If there is a simpler, safer, or more scalable path, say so.

Do not over-explain obvious implementation details. Do explain architectural decisions, risks, and tradeoffs.
