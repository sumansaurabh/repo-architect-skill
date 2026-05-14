# Project Claude Routing

## Available Local Skills

- `/repo-onboarding`: primary entrypoint for end-to-end repository onboarding. Generates a multi-file `arch-YYYY-MM-DD-<repo-slug>/` pack covering stack, architecture, code map, build/deploy, gotchas, and a 30/60/90 ramp plan.
- `/repo-deep-dive`: focused tour of one subsystem, service, or request path within a repo. Smaller pack, written as a hands-on code reading.
- `/repo-cross-exam`: pressure-tests a new hire's understanding with week-2 verification questions and grounded model answers.

## Routing Rules

When the user asks to be onboarded to a repo, walked through a codebase end-to-end, or wants an architecture overview for a new hire — invoke `/repo-onboarding`.

When the user already has an onboarding pack and wants to dig into one slice (a service, module, request path, or domain area) — invoke `/repo-deep-dive`.

When the user wants to verify their understanding of a repo through challenge questions, mock interview pressure on the codebase, or week-2 self-check material — invoke `/repo-cross-exam`.

## Repo Conventions

- Primary context for every skill is the target repository's actual files. Always read the manifests (`package.json`, `pyproject.toml`, `go.mod`, etc.), top-level README, CI config, and entry points before claiming anything about the stack.
- Generated outputs go under `arch-packs/arch-YYYY-MM-DD-<repo-slug>/` and must include `manifest.json`.
- Supported archetypes and required file sets are defined in `arch-packs/README.md`.
- Reuse a pack only when the folder is explicitly named by the user. Date-stamped folders are immutable snapshots.
- Use the eight-lane default bundle from `/repo-onboarding`: stack, architecture, data, APIs, build/deploy, code map, gotchas, ramp plan.
- Keep claims grounded. Cite file paths with line ranges. If a claim has no evidence in the repo, omit it or mark it as an assumption.
