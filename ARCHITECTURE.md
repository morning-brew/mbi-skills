# mbi-skills — Architecture & Coexistence

How MBI skills share resources, stay independent, and compose safely when a user has multiple installed.

## Integration kinds

A skill declares its integrations in `manifest.json`. Each declaration has a `kind`:

| kind | What it means | Who owns the credential | Stored where |
|---|---|---|---|
| `bundled-oauth-client` | Skill ships a public OAuth client_id/secret; BrewCrew runs consent once and stores a per-user refresh token. | BrewCrew | Keychain under `brewcrew.<skill-id>.<cred-name>` |
| `mcp-oauth` | The skill adds an entry to the CLI's MCP config. The MCP server handles its own auth flow. | MCP server binary | MCP's own private store — never in BrewCrew |
| `user-provided-key` | Skill declares fields (e.g. `JAMF_API_USER`); install flow prompts user to paste. | BrewCrew | Keychain, skill-scoped |
| `cli-auth-prereq` | Skill delegates auth to a third-party CLI (e.g. `gws`, `gh`). BrewCrew ensures the binary is installed and surfaces "run `<binary> auth login`" when unauthed. | The CLI itself | CLI's own config (e.g. `~/.config/gws/`) |

## Shared resources & coexistence rules

### CLI binaries (`gws`, `gh`, `rg`, …)

- **Reference-counted** across installed skills. Installing a second skill that needs `gws` is a no-op for the binary. Uninstalling a skill only removes the binary when no other installed skill lists it in `tools.required_binaries`.
- Installation: Phase 5 installer invokes `brew install <binary>` if missing.
- No skill should assume the binary is there — every skill's `skill.md` gracefully handles the "not installed" case.

### CLI-managed OAuth scopes

When multiple `cli-auth-prereq` skills share a CLI (e.g. both calendar-assistant and ramp-assistant use `gws`), the required scopes are the **union** across all installed skills.

- At install time, Phase 5 re-runs `<cli> auth login --scopes <union>` to re-consent with the wider scope set.
- At uninstall time, scopes are **left as-is** rather than narrowed (narrowing requires forced re-consent which is disruptive for a single skill change). This is a deliberate tradeoff: slightly over-broad permission is preferred over a noisy uninstall flow.

### MCP servers

- `mcp_config.apply()` in the daemon merges the MCP servers from all *active* skills (intersection of installed + trigger-matched) into the CLI's MCP config, marking each entry with `source: brewcrew-skill:<id>`.
- On uninstall, only that skill's entries are removed. User-authored MCP entries are never touched.
- On collision (two skills declaring the same MCP `id`), last-writer-wins — warn in the log but don't fail. Skills should use distinct ids anyway.

### Keychain entries

- All BrewCrew-managed credentials live under `brewcrew.<skill-id>.<cred-name>`.
- Uninstalling a skill deletes its namespace. No other skill's keys are touched.

### Previous chat sessions

- Each session's message history is stored in SQLite unchanged by skill install state.
- The `active_skills` list attached to a session is derived per-turn from the *currently installed* skill set. Old turns render with whatever skill chips fired at the time (not re-computed).
- If a user asks Claude to "redo that" after uninstalling a skill, the new turn won't activate the removed skill — and Claude will say "I can't do X anymore because the <name> skill isn't installed." That's the correct UX.

### Scope widening / removal edge cases

- **Install skill A**, consent with A's scopes, use it.
- **Install skill B** that requires additional scopes. Phase 5 triggers re-consent with the union; if the user declines the new scopes, skill B installs but reports `needs-setup` until consent succeeds.
- **Uninstall skill A**, keep skill B. Keychain entries for A are cleared; MCP entries for A reverted. `gws` scopes stay wide (see trade-off above). The user never loses B's functionality.
- **Install both, then uninstall both.** MCP config returns to its pre-skill state. CLI OAuth tokens remain (user can `gws auth revoke` manually if they want).

## Authoring guidance for skill authors

- Declare **every** binary you use in `tools.required_binaries` so ref-counting works.
- Declare **every** scope you need — don't rely on another skill having granted them.
- Never write to the keychain directly; declare a `user-provided-key` or `bundled-oauth-client` integration and let the installer handle it.
- MCP server ids should be globally unique and descriptive (e.g. `ramp-expenses`, not `mcp`).
- Knowledge routes point at **live sources** (Notion URLs, Confluence pages). Don't cache content in the skill package — it rots.
- Your skill should run correctly with **only itself** installed. Don't depend on another skill's context.

## Current status

- Phase 4 skill loader + keyword matcher + prompt assembler + MCP config writer: all in BrewCrew v2.
- Phase 3 credential handlers (actually running install flows per kind): in progress. `cli-auth-prereq` handler planned to be a no-op at install (installer still runs `brew install` + `<cli> auth login`) and a status check at runtime.
- Phase 5 marketplace installer with ref-counting: not yet implemented. Today `mbi-skills` is cloned into `~/.brewcrew/cache/mbi-skills/` and discovered by the daemon without marketplace UI.
