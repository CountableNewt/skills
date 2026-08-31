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

### notion-project-documentation

**When to use:** When the user wants to create, update, or store project documentation such as docs, wiki pages, specs, ADRs, runbooks, guides, FAQs, notes, or knowledge capture.

**What it does:** Routes project documentation into Notion subpages under the project page through the Notion MCP server. Searches for the correct parent page first, updates existing matching docs when possible, and asks the user to choose the parent page when it is ambiguous or missing.

**Path:** [notion-project-documentation/SKILL.md](notion-project-documentation/SKILL.md)

---

### skills-contribution-workflow

**When to use:** When improving or suggesting changes to skills in this repo, when a conversation implies a new skill, when choosing project-local vs central placement, or when authoring with MyContextProtocol/MCP in mind.

**What it does:** Routes proposals through Linear under the Skills team (issues sync into the repo; agents do not use GitHub for issue tracking). Classifies whether a skill stays in a consumer repo or belongs in countablenewt/skills. Reminds authors that skills here are published via MyContextProtocol as MCP endpoints and should read as stable contracts.

**Path:** [skills-contribution-workflow/SKILL.md](skills-contribution-workflow/SKILL.md)

---

### swift-strict-concurrency

**When to use:** When working in Swift 6 mode, strict concurrency, or warnings-as-errors workflows for Vapor/server-side Swift or iOS/Xcode Swift projects.

**What it does:** Enforces warning-free Swift work under strict concurrency. Agents must not introduce warnings, must treat surfaced warnings as in scope, must prefer actor isolation and `Sendable` fixes over suppression, and must track warnings they cannot safely resolve inline.

**Path:** [swift-strict-concurrency/SKILL.md](swift-strict-concurrency/SKILL.md)

---

### ui-grammar

**When to use:** When reviewing, writing, or fixing user-facing interface text, especially labels, buttons, headings, and count-driven copy.

**What it does:** Applies consistent title case to interface labels and ensures singular, plural, and verb agreement are derived from runtime counts.

**Path:** [ui-grammar/SKILL.md](ui-grammar/SKILL.md)

---

## When working in this repo

- **New skills:** Create `skill-name/SKILL.md` with standard Agent Skills YAML frontmatter (`name`, `description`) and markdown instructions.
- **Conventions:** Follow [linear-workflow/SKILL.md](linear-workflow/SKILL.md).
- **Descriptions:** Keep them specific and include trigger terms so agents know when to apply the skill.
- **Runtime policy:** Put MyContextProtocol routing and enforcement in `.mycontext/skills.yaml`; keep `SKILL.md` portable.
- **Updates:** Add new skills to the catalog in this file and to the README.

## Pulling skills to other projects

When helping users install skills from this repo:

1. Copy the complete skill directory into `.agents/skills/<skill-name>/` for a project, or into the agent's supported global skill directory.
2. Restart or reload the consuming agent if it does not watch skill directories.

MyContextProtocol also exposes the packages as MCP resources for agents that do not install them locally.
