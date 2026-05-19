# Vibe Code Starter

A reusable Claude Code starter kit for AI-assisted software projects.

## What this repo is for

Use this as a GitHub template repo. Every new project created from this template starts with:

- `CLAUDE.md` — repo-level Claude Code instructions
- `PROJECT_BRIEF.md` — project intake and source of truth
- `BUILD_PLAN.md` — phased execution plan and checkpoints
- `DECISIONS.md` — architecture and product decision log
- `REVIEW_CHECKLIST.md` — quality, security, testing, and release checklist
- `ai-methodology/` — reusable best-practice modules

## New project workflow

1. Create a new repo from this template.
2. Open the repo in VS Code.
3. Start Claude Code from the project root.
4. Tell Claude:

```text
Read CLAUDE.md and run the project setup workflow before writing code.
```

5. Answer the intake questions.
6. Review and approve the proposed `PROJECT_BRIEF.md` and `BUILD_PLAN.md`.
7. Only then begin implementation.

## Core principle

Do not use one massive prompt. Use a small repo-level instruction file plus focused methodology modules. Claude should load the minimum context needed for the current phase.
