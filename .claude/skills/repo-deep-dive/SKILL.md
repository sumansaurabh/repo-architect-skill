---
name: repo-deep-dive
description: |
  Targeted deep-dive into one slice of a codebase — a service, subsystem, request
  path, or domain module. Produces a focused multi-file pack with architecture,
  control flow, data model, failure modes, and a hands-on tour, written as if a
  staff engineer were walking a new hire through that exact area.
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Agent
  - AskUserQuestion
  - Bash
triggers:
  - deep dive into this module
  - walk me through the auth service
  - explain this subsystem
  - trace this request end to end
  - generate a subsystem onboarding pack
---

## When To Use

Use this skill when the user already has an onboarding overview (from
`/repo-onboarding`) and wants a *focused* tour of one area — e.g. "explain the
billing service", "walk me through the payments webhook flow", "deep dive on the
search indexer".

The output is a smaller, focused pack — not a full repo onboarding.

## Inputs To Read First

- the pack folder `arch-packs/arch-YYYY-MM-DD-<repo-slug>/` if one exists for this
  repo (read `manifest.json` and the code map)
- the specific module or directory the user named
- any relevant docs under `docs/**` for that area
- the most recent ~30 commits touching that directory (`git log --oneline -- <path>`)

If the target area is ambiguous, ask one short clarifying question naming the
candidate directories you found.

## Output Folder

`arch-packs/arch-YYYY-MM-DD-<repo-slug>/deep-dives/<area-slug>/`

If no parent onboarding pack exists, create one folder at:

`arch-packs/arch-YYYY-MM-DD-<repo-slug>-<area-slug>/`

## Required Files

- `README.md` — what this deep-dive covers and what it deliberately skips.
- `manifest.json` — extends the parent manifest with `archetype: "deep-dive"`,
  `parentPack`, and `targetPath`.
- `00-scope-and-context.md` — what's in scope, what's out, why this area matters.
- `01-mental-model.md` — the one paragraph and one diagram that unlock the area.
- `02-architecture.md` — internal components, dependencies in and out.
- `03-control-flow.md` — sequence diagram for the main happy-path request.
- `04-data-model.md` — schemas, key invariants, lifecycle of the core entity.
- `05-failure-modes.md` — what breaks, how it's detected, how it's recovered.
- `06-extension-points.md` — where you'd add new behavior, and what to avoid touching.
- `07-hands-on-tour.md` — a 20-minute guided code reading: file by file, line by line.
- `08-common-tasks.md` — recipes for the 5 most common changes in this area.

## Parallel Lanes

When Agent is available, split into:

1. internal architect — components and boundaries
2. control-flow tracer — request paths and sequence diagrams
3. data modeler — entities, schemas, invariants
4. failure-mode auditor — what breaks and how
5. code-reading guide — file-by-file tour for a new hire

## Quality Bar

- Cite file paths with line ranges everywhere a claim is made.
- Show one Mermaid sequence diagram for the main flow.
- "Common tasks" must be concrete enough that a new hire could attempt one same-day.
- Be honest about messy areas. Mark them with `> ⚠ messy:` rather than glossing over.

## Guardrails

- Do not duplicate the parent onboarding pack — link to it.
- Do not invent contracts. Quote the code.
- Do not skip failure modes — they are the highest-value section after control flow.
