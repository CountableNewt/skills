# Skills

Central repository for skills used across projects. Track, version, and pull them for other work.

## What are skills?

Skills are markdown files that teach AI agents specialized workflows. Each skill lives in a directory with a `SKILL.md` file containing YAML frontmatter and instructions. Compatible with Cursor and Codex.

## Skills in this repo

| Skill | Description | Path |
|-------|-------------|------|
| linear-workflow | Linear as source of truth for planning and status. Check branch for Linear ID, set In Progress, add comments during work. Use before coding or planning. | `linear-workflow/` |

## Using skills in other projects

**Current:** Copy the skill folder into your target location:

- **Project-scoped:** `.cursor/skills/<skill-name>/`
- **Global:** `~/.cursor/skills/<skill-name>/`

Example:
```bash
cp -r linear-workflow ~/.cursor/skills/
```

**Future:** A custom MCP server will provide centralized installation.

## Skill format

Each skill is a directory with a `SKILL.md` file:

```
skill-name/
└── SKILL.md
```

`SKILL.md` has YAML frontmatter (`name`, `description`) and a markdown body with instructions. See [linear-workflow/SKILL.md](linear-workflow/SKILL.md) for the convention.

## Adding skills

1. Create a new directory: `skill-name/`
2. Add `SKILL.md` with frontmatter and instructions
3. Follow conventions from [linear-workflow/SKILL.md](linear-workflow/SKILL.md)
4. Update this README and [AGENTS.md](AGENTS.md) with the new skill
