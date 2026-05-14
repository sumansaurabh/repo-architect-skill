---
name: repo-cross-exam
description: |
  Pressure-test a new-hire's understanding of the codebase. Generates the kinds of
  questions a tech lead would ask in week 2 to find gaps: "what happens if X
  fails?", "where would you add Y?", "why is Z this way and not that way?". Pairs
  with `/repo-onboarding` packs.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Agent
  - AskUserQuestion
---

## When To Use

Use this skill when a new hire (or returning engineer) wants to verify they
actually understood the onboarding pack, not just read it. The output is a set of
challenge questions with grounded model answers.

The input can be:

- an existing onboarding pack folder, or
- a repo path with no pack yet (in which case run `/repo-onboarding` first)

## Output Folder

If a parent pack exists: `arch-packs/arch-YYYY-MM-DD-<repo-slug>/cross-exam/`

If no parent exists, first generate the base pack with `/repo-onboarding`, then add
the cross-exam expansion under that pack's `cross-exam/` folder.

## Required Files

- `cross-exam/README.md` — scope and how it maps to the parent pack.
- `cross-exam/architecture-pushback.md` — "why is it this way?" questions on
  service boundaries and dependencies.
- `cross-exam/control-flow-traps.md` — request flow questions with subtle bugs
  baked in; can the reader spot them?
- `cross-exam/data-and-failure.md` — schema invariants, race conditions, what
  breaks under partial failure.
- `cross-exam/change-scenarios.md` — "you've been asked to add X. Where do you
  start? What do you touch? What breaks?"
- `cross-exam/onboarding-self-check.md` — 20 short questions a new hire should be
  able to answer after week 1, with model answers.
- `cross-exam/fast-rebuttals.md` — one-paragraph answers to the hardest questions.

## Parallel Lanes

When Agent is available, split into:

1. skeptical architect — boundary and abstraction questions
2. request-flow inspector — happy path vs edge case
3. data integrity auditor — invariants and race conditions
4. change-impact predictor — blast radius of common edits
5. tech-lead interviewer — week-2 verification questions

## Quality Bar

- Every question must be answerable from the actual repo. Cite paths in the
  model answer.
- Include at least one "trap" question per major subsystem (a plausible-sounding
  wrong answer to flag).
- Include at least one change-impact scenario per service.
- Answers should be crisp — under 150 words each.
- **All diagrams must be Mermaid.** No ASCII art, no ANSI box-drawing. Use
  Mermaid `sequenceDiagram` blocks in `control-flow-traps.md` to set up the
  scenario, and `flowchart` blocks in `change-scenarios.md` to show blast
  radius of the proposed change.

## Guardrails

- Do not put cross-exam files in the numbered root sequence of the parent pack.
  They belong under `cross-exam/`.
- Do not generate questions that the parent pack does not have the evidence to
  answer.
- Do not write a question without writing the model answer alongside it.
- Do not use ASCII art or text-based pseudo diagrams. Every diagram is a
  fenced ```mermaid``` block.
