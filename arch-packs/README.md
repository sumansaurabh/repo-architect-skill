# Arch Packs

Generated repo-onboarding outputs live here.

## Naming

Use a folder name shaped like:

`arch-packs/arch-YYYY-MM-DD-<repo-slug>/`

Example:

`arch-packs/arch-2026-05-14-resume-skiller/`

The prefix is always `arch-` followed by today's date in ISO format, then the
kebab-cased basename of the repo being onboarded. If multiple packs are generated
for the same repo on the same day, append `-v2`, `-v3`, etc.

## Required Metadata

Every pack folder must include a machine-readable `manifest.json`.

Use this shape:

```json
{
  "schemaVersion": 1,
  "archetype": "repo-onboarding",
  "slug": "resume-skiller",
  "createdAt": "2026-05-14",
  "repoPath": "/Users/sumansaurabh/Documents/startup-3/resume-skiller",
  "primarySkill": "/repo-onboarding",
  "sourceFiles": ["README.md", "package.json", "..."],
  "stack": {
    "languages": ["TypeScript", "Python"],
    "frameworks": ["Next.js"],
    "datastores": ["PostgreSQL"]
  },
  "grounding": {
    "confidence": "high",
    "filesSampled": 47,
    "notes": "Sampled top-level + each service entry point + all CI workflows."
  }
}
```

## Supported Archetypes

### `repo-onboarding`

Use for end-to-end onboarding of a single repository.

Required root files:

- `README.md`
- `manifest.json`
- `00-quick-start.md`
- `01-30-second-pitch.md`
- `02-stack-and-dependencies.md`
- `03-architecture.md`
- `04-code-map.md`
- `05-data-and-storage.md`
- `06-apis-and-integrations.md`
- `07-build-test-deploy.md`
- `08-config-and-secrets.md`
- `09-observability-and-debugging.md`
- `10-gotchas-and-tribal-knowledge.md`
- `11-glossary.md`
- `12-ramp-plan.md`
- `13-people-and-ownership.md`

Optional root files when the repo warrants them:

- `14-frontend-walkthrough.md`
- `15-backend-walkthrough.md`
- `16-ml-and-models.md`
- `17-mobile-walkthrough.md`
- `18-security-model.md`
- `19-performance-notes.md`
- `20-faq.md`

### `deep-dive`

Use for focused tours of a single subsystem. Lives under
`arch-packs/<parent-pack>/deep-dives/<area-slug>/` when a parent pack exists.

Required root files:

- `README.md`
- `manifest.json`
- `00-scope-and-context.md`
- `01-mental-model.md`
- `02-architecture.md`
- `03-control-flow.md`
- `04-data-model.md`
- `05-failure-modes.md`
- `06-extension-points.md`
- `07-hands-on-tour.md`
- `08-common-tasks.md`

## Cross-Exam Expansion

Extended challenge material lives under `cross-exam/` inside the parent pack, not
in the numbered root sequence.

## Snapshot Discipline

Each pack is dated and immutable. If the codebase changes meaningfully, generate a
new pack on today's date. Do not overwrite an old pack — readers may be mid-ramp
against it.
