# Agent Instructions

This repo is a central skills repository. Use it to discover and apply skills when working in projects that pull from it.

## Skills catalog

### linear-workflow

**When to use:** Before any coding task, fix, or implementation; when in plan mode; when switching branches; when the user asks you to implement, fix, change something, or create a plan.

**What it does:** Linear is the source of truth for planning, status, blockers, and verification. Check branch for Linear ID, confirm issue exists, set In Progress, add comments during work, keep Linear updated. Do not skip—invoke before touching files.

**Path:** [linear-workflow/SKILL.md](linear-workflow/SKILL.md)

---

### supabase-best-practices

**When to use:** When work touches Supabase or Postgres schema design, migrations, `supabase db` flows, normalization, or local reset/init behavior.

**What it does:** Enforces Postgres-first schema design, pushes for BCNF or at least 3NF, requires committed migrations for schema changes, and requires local init/reset flows to apply migrations reproducibly.

**Path:** [supabase-best-practices/SKILL.md](supabase-best-practices/SKILL.md)

---

## When working in this repo

- **New skills:** Create `skill-name/SKILL.md` with YAML frontmatter (`name`, `description`) and markdown instructions.
- **Conventions:** Follow [linear-workflow/SKILL.md](linear-workflow/SKILL.md).
- **Descriptions:** Keep them specific and include trigger terms so agents know when to apply the skill.
- **Updates:** Add new skills to the catalog in this file and to the README.

## Pulling skills to other projects

When helping users install skills from this repo:

1. Copy the skill directory into `.cursor/skills/<skill-name>/` (project) or `~/.cursor/skills/<skill-name>/` (global).
2. Restart Cursor to pick up new skills.

**Future:** An MCP server will provide centralized installation.
