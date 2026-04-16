# mbi-skills

MBI-specific skill registry for BrewCrew v2. Each skill is a directory under `skills/` containing a `manifest.json`, `skill.md`, and optional MCP server / escalation / knowledge-route files. BrewCrew clones this repo into `~/.brewcrew/cache/mbi-skills/` and merges the index into the marketplace view.

## Layout

```
mbi-skills/
├── index.json               # list of available skills with metadata
└── skills/
    ├── mbi-onboarding/
    │   ├── manifest.json
    │   ├── skill.md
    │   ├── mcp-servers.json     # optional
    │   ├── escalation.json      # optional
    │   └── knowledge-routes/    # optional
    │       └── *.md
    └── ...
```

## Manifest schema

See the BrewCrew v2 Phase 4 plan (`brewcrew-ai/docs/superpowers/plans/2026-04-16-brewcrew-v2-phase-4-skills.md`) for the authoritative schema. Each `manifest.json` must have:

- `id` — globally unique, lowercase, kebab-case
- `name`, `version`, `description`
- `always_active` (bool)
- `triggers` — list of keywords (substring-matched case-insensitively against user messages)
- `integrations` — list of integration IDs from `config/integrations.json` the skill needs

## Index schema

`index.json` at the root is what BrewCrew reads to populate the marketplace:

```json
{
  "registry": "mbi-skills",
  "version": 1,
  "skills": [
    {
      "id": "mbi-onboarding",
      "name": "MBI Onboarding",
      "version": "0.1.0",
      "author": "MBI IT",
      "description": "New-hire provisioning runbook: Okta account, Google Workspace, Jamf enrollment, Slack invites.",
      "required_integrations": ["okta", "gws", "jamf", "slack"],
      "path": "skills/mbi-onboarding"
    }
  ]
}
```

## Contributing

1. Add your skill directory under `skills/<id>/`
2. Add an entry to `index.json`
3. Open a PR — a maintainer (see `skills/<id>/MAINTAINERS.md`) must approve

## Ownership

Each skill has a named maintainer who is responsible for keeping routes fresh and the escalation rules aligned with real MBI workflows. See individual `MAINTAINERS.md` files inside each skill.

## Related

- BrewCrew v2 repo: https://github.com/morning-brew/brewcrew-ai
- Skill format spec: `brewcrew-ai/docs/superpowers/plans/2026-04-16-brewcrew-v2-phase-4-skills.md`
