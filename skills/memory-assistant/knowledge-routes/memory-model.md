# How BrewCrew memory works

## Where to look

- `~/.brewcrew/memory/` on the user's machine. Everything is local + markdown.
- No cloud sync by default. Users can point this at a private git repo if they want backup.

## Design principles (borrowed from gbrain)

- **Compiled truth + timeline** — every memory file has a mutable synthesis above a divider and an append-only evidence trail below. Prevents losing history while staying readable.
- **Brain-first lookup** — read memory *before* hitting external APIs or inventing context.
- **Repo-as-source-of-truth** — markdown in a known directory. Users can open, edit, and commit these files themselves. No vendor lock-in.

## When to use

- Every turn, if it mentions a named entity (person, project, workflow) we might know.
- Also capture-time: at the end of every turn, decide whether there's a durable fact worth stashing.

## How to act

- Keep files small. If Compiled truth is growing past a screen, you're over-writing.
- Cite memory when you use it: *"based on what you said on 2026-04-12, you use Slack DMs for quick questions."*
- Never fabricate memory. If there's no file, there's no memory — don't invent one.
