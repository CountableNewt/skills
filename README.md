# Skills

Central repository for skills used across projects. Track, version, and pull them for other work.

## What are skills?

Skills are markdown files that teach AI agents specialized workflows. Each skill lives in a directory with a `SKILL.md` file containing YAML frontmatter and instructions. They are compatible with agents that implement the Agent Skills specification, including Codex and Cursor. MyContextProtocol publishes the same packages as MCP resources without rewriting their standard `SKILL.md` files.

## Skills in this repo

| Skill | Description | Path |
|-------|-------------|------|
| linear-workflow | Linear as source of truth for planning and status. Check branch for Linear ID, set In Progress, add comments during work. Use before coding or planning. | `linear-workflow/` |
| notion-project-documentation | Route project documentation into Notion subpages under the project page. Search for the right parent page, update matching docs when they exist, and ask when the parent is ambiguous. | `notion-project-documentation/` |
| skills-contribution-workflow | Propose skill changes and new skills via Linear (Skills team); classify local vs central skills; note MyContextProtocol MCP exposure. | `skills-contribution-workflow/` |
| supabase-best-practices | Supabase and Postgres schema discipline. Prefer BCNF or 3NF, require committed migrations, and keep local init/reset reproducible through migrations. | `supabase-best-practices/` |
| swift-strict-concurrency | Swift 6 strict concurrency and warnings-as-errors discipline for Vapor/server-side Swift and iOS/Xcode projects. Keep build output warning-free, fix surfaced warnings inline when safe, and track follow-up work when they cannot be resolved in-task. | `swift-strict-concurrency/` |
| ui-grammar | Review and improve user-facing interface copy, including title case and count-sensitive grammar. | `ui-grammar/` |

## Using skills in other projects

**Current:** Copy the skill folder into your target location:

- **Project-scoped:** `.agents/skills/<skill-name>/`
- **Codex global:** `~/.codex/skills/<skill-name>/`
- **Cursor global:** `~/.cursor/skills/<skill-name>/`

Example:
```bash
mkdir -p .agents/skills
cp -r linear-workflow .agents/skills/
```

Alternatively, connect MyContextProtocol to discover these packages as MCP resources. Agents should resolve task context before reading only the package files they need.

## Skill format

Each skill is a directory with a `SKILL.md` file:

```
skill-name/
└── SKILL.md
```

`SKILL.md` has YAML frontmatter (`name`, `description`) and a markdown body with instructions. See [linear-workflow/SKILL.md](linear-workflow/SKILL.md) for the convention.

Repository-level runtime routing lives in `.mycontext/skills.yaml`. The sidecar adds activation, scope, requirements, conflicts, and negative-routing policy without adding proprietary fields to the portable skill package.

## Adding skills

1. Create a new directory: `skill-name/`
2. Add `SKILL.md` with frontmatter and instructions
3. Follow conventions from [linear-workflow/SKILL.md](linear-workflow/SKILL.md)
4. Update this README and [AGENTS.md](AGENTS.md) with the new skill
