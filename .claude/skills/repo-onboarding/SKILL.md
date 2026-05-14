---
name: repo-onboarding
description: |
  Primary repository-onboarding orchestrator. Walks a codebase end-to-end the way a
  staff engineer would brief a new hire on day one: entry points, services, data flow,
  build and run loops, deploy path, gotchas, and a 30/60/90 ramp plan. Fans the work
  across parallel agent lanes and writes a multi-file architecture pack under
  `arch-packs/arch-YYYY-MM-DD-<repo-slug>/`.
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
  - onboard me to this repo
  - explain this repository end to end
  - help me understand this codebase
  - new hire walkthrough
  - architecture overview of this repo
  - generate onboarding pack
---

## Purpose

Use this as the default skill whenever a user (typically a new joiner, transferring
engineer, or someone returning to a repo after months away) asks to be brought up to
speed on a codebase. The output should feel like a senior engineer wrote a private
onboarding doc for a single new hire — not a generic README.

The skill must not stop at a chat answer. By default it creates a reusable multi-file
markdown pack under `arch-packs/arch-YYYY-MM-DD-<repo-slug>/` unless the user
explicitly asks not to. The folder prefix is always `arch-` followed by the current
date in ISO format.

## Inputs To Read First

Always scan (do not read every file — sample strategically):

- root `README*`, `CONTRIBUTING*`, `ARCHITECTURE*`, `CHANGELOG*`, `CLAUDE.md`, `AGENTS.md`
- manifest files: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`,
  `build.gradle*`, `Gemfile`, `composer.json`, `requirements*.txt`
- container and CI: `Dockerfile*`, `docker-compose*.yml`, `.github/workflows/**`,
  `.gitlab-ci.yml`, `Jenkinsfile`, `.circleci/**`, `fly.toml`, `vercel.json`,
  `netlify.toml`, `render.yaml`, `Procfile`, `serverless.yml`
- env templates: `.env.example`, `.env.sample`, `config/`, `settings*`
- entry points: `main.*`, `index.*`, `app.*`, `cmd/**`, `bin/**`, `server.*`,
  `routes/**`, `api/**`, `handlers/**`
- migrations and schema: `migrations/**`, `schema.sql`, `prisma/**`, `*.proto`
- top-level dirs: list them, then sample 1–3 representative files from each

Read when present:

- `docs/**` directory tree (sample the index plus the most-linked docs)
- existing `arch-packs/` folders (only attach if an explicit folder name is given)

## Workflow

1. Determine the target repo path. Default to the current working directory unless
   the user names another path.
2. Compute the repo slug: lowercased basename of the repo root, kebab-cased.
3. Compute the pack folder: `arch-packs/arch-YYYY-MM-DD-<repo-slug>/` using today's
   date. If the user passes a custom slug, use it instead.
4. Scan inputs above. Build a mental model of stack, services, and boundaries.
5. If the codebase is ambiguous (e.g. multiple apps in a monorepo), ask one short
   clarifying question about scope before fanning out.
6. Write `manifest.json` first.
7. Fan out the parallel agent lanes (see below) when Agent is available.
8. Synthesize each lane's output into its target file. Keep files focused — one
   concern per file.
9. Return a short summary with the pack folder path and the 30-second pitch.

## Parallel Agent Lanes

Use parallel agents whenever Agent is available. Default lanes:

1. **Stack & dependencies lane** — language, runtime, frameworks, key libraries,
   versions, and what each is doing.
2. **Architecture lane** — service map, request flow, control vs data plane,
   internal boundaries, and a Mermaid topology diagram.
3. **Data lane** — primary datastores, schema highlights, migrations, caching,
   and external data sources.
4. **API & integration lane** — public APIs, internal contracts, queues, webhooks,
   third-party integrations.
5. **Build & deploy lane** — how to install, run locally, test, lint, build,
   deploy; CI/CD; environments; secrets handling.
6. **Code map lane** — directory tree with one-line purpose per folder, plus the
   ~10 files a new hire should read first in order.
7. **Gotchas & tribal knowledge lane** — non-obvious behaviors, footguns, recent
   incidents (mine `CHANGELOG`, `git log`, issue templates), and known tech debt.
8. **Ramp plan lane** — 30/60/90-day plan with concrete first PRs of escalating
   scope, suggested mentor pairings, and reading checkpoints.

If Agent is unavailable, do the same reasoning sequentially and note the fallback
in the manifest grounding section.

## Required Output Files

For an onboarding pack, create at minimum:

- `README.md` — one-screen overview, 30-second pitch, file map of the pack.
- `manifest.json` — pack metadata.
- `00-quick-start.md` — clone, install, run, test, common commands. Copy-paste ready.
- `01-30-second-pitch.md` — what this repo is, who uses it, why it exists.
- `02-stack-and-dependencies.md` — languages, frameworks, runtimes, key libraries.
- `03-architecture.md` — service map and request flow with Mermaid diagrams.
- `04-code-map.md` — directory tree with one-line purpose per folder; "read these
  files first" ordered list.
- `05-data-and-storage.md` — datastores, schema highlights, migrations, caching.
- `06-apis-and-integrations.md` — public APIs, queues, webhooks, third-party deps.
- `07-build-test-deploy.md` — local dev loop, CI, deploy path, environments.
- `08-config-and-secrets.md` — env vars, config files, where secrets live, how they
  flow at runtime.
- `09-observability-and-debugging.md` — logs, metrics, traces, common error patterns,
  how to debug a failing request.
- `10-gotchas-and-tribal-knowledge.md` — footguns, non-obvious behaviors, "here be
  dragons" zones.
- `11-glossary.md` — domain terms and acronyms in plain English.
- `12-ramp-plan.md` — 30/60/90-day plan with concrete first PR suggestions.
- `13-people-and-ownership.md` — code ownership from `CODEOWNERS` / `git log`;
  who to ask about which area.

Optional files when the repo warrants them:

- `14-frontend-walkthrough.md`
- `15-backend-walkthrough.md`
- `16-ml-and-models.md`
- `17-mobile-walkthrough.md`
- `18-security-model.md`
- `19-performance-notes.md`
- `20-faq.md`

## Manifest Shape

Write this as the first file:

```json
{
  "schemaVersion": 1,
  "archetype": "repo-onboarding",
  "slug": "<repo-slug>",
  "createdAt": "YYYY-MM-DD",
  "repoPath": "<absolute path scanned>",
  "primarySkill": "/repo-onboarding",
  "sourceFiles": ["README.md", "package.json", "..."],
  "stack": {
    "languages": ["..."],
    "frameworks": ["..."],
    "datastores": ["..."]
  },
  "grounding": {
    "confidence": "high|medium|low",
    "filesSampled": 0,
    "notes": "explain any gaps"
  }
}
```

## Grounding Standard

- Treat the actual files in the repo as the only source of truth. Never invent
  endpoints, models, or services that you did not see.
- When you cannot find evidence for a claim (e.g. "this service handles 10k RPS"),
  either omit it or mark it explicitly as an assumption.
- Quote file paths with line ranges when describing specific behavior, e.g.
  `src/server.ts:42-71`.
- If sampling was shallow, lower `grounding.confidence` and say why in the manifest.

## What Good Looks Like

The output should feel like a staff engineer's private onboarding doc for one new
hire:

- written in second person ("you'll first run...", "you'll notice...")
- direct, not encyclopedic — link to code instead of repeating it
- explicit about what to read first, second, third
- explicit about what *not* to touch in week one
- includes a real local-dev path that actually works end to end
- includes diagrams where topology matters
- includes a concrete 30/60/90 plan with first-PR candidates
- separates "what is true" from "what we recommend"
- captures the non-obvious stuff: deploy quirks, flaky tests, owned-by-X areas

## Writing Rules

- Markdown headings and short paragraphs. Tables for inventories.
- **All diagrams must be Mermaid.** No ASCII art, ANSI box-drawing, or
  pseudo-graphical text. If a topology, sequence, state, ER, or flow needs a
  picture, use a fenced ```mermaid``` block. This applies to every file in the
  pack, not just `03-architecture.md`.
- Use the right Mermaid diagram type for the job:
  - `flowchart LR` / `flowchart TD` — service topology, request flow, component map
  - `sequenceDiagram` — request lifecycle, control flow, multi-service handshakes
  - `erDiagram` — data model and entity relationships
  - `stateDiagram-v2` — entity lifecycle, job state machines
  - `classDiagram` — module/class structure when relevant
  - `gantt` — 30/60/90 ramp plan timeline
  - `journey` — new-hire user journey through the codebase
- Every numbered file should include at least one Mermaid diagram where a
  picture clarifies the prose. Specifically:
  - `03-architecture.md` — at least one `flowchart` of the service topology
    AND one `sequenceDiagram` of the main request flow
  - `05-data-and-storage.md` — at least one `erDiagram` of the core entities
  - `06-apis-and-integrations.md` — `sequenceDiagram` for any non-trivial flow
    (auth, payment, webhook)
  - `07-build-test-deploy.md` — `flowchart` of the CI/CD pipeline from commit
    to production
  - `09-observability-and-debugging.md` — `flowchart` of the logs/metrics/traces
    pipeline
  - `12-ramp-plan.md` — `gantt` chart of the 30/60/90 plan
- Code fences for every shell command — copy-paste must work.
- Always include file paths with line numbers when describing behavior.
- Tone: direct, technical, day-one-friendly. No marketing voice.

## Mermaid Quality Bar

- Keep node labels short — 1–4 words. Long labels break rendering.
- Use subgraphs to group services by deployment unit or domain boundary.
- Label edges with the verb that describes the call (`POST /jobs`, `publishes`,
  `reads`, etc.) — never leave an edge unlabeled in a sequence diagram.
- Render the diagram mentally before writing it: if it would have more than ~15
  nodes, split it into two diagrams.
- Use `note over` / `note right of` in sequence diagrams to call out invariants,
  retries, or idempotency keys.

## Failure Modes To Avoid

- Do not produce one monolithic markdown file. Use the file split above.
- Do not fabricate endpoints, jobs, or services. Only describe what you saw.
- Do not paste large chunks of source code verbatim — link to the file path
  with line numbers instead.
- Do not use ASCII art, ANSI box-drawing characters, or text-based pseudo
  diagrams. Every diagram must be a fenced ```mermaid``` block.
- Do not skip the ramp plan. A pack without a ramp plan is not an onboarding pack.
- Do not skip the gotchas file. Tribal knowledge is the highest-value section.
- Do not assume a stack — read the manifests before claiming a framework.
- Do not reuse a previous pack folder unless the user named it explicitly.
