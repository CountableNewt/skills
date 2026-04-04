---
name: skills-contribution-workflow
description: Routes skill feedback and new-skill ideas through Linear under the Skills team; classifies project-local vs central-repo skills; reminds authors that countablenewt/skills content is published via MyContextProtocol as MCP endpoints. Use when improving a skill, proposing a new skill from conversation, contributing upstream, or choosing repo-specific vs shared placement.
---

# Skills contribution and MCP context

Use this skill when a conversation surfaces something that should change how agents work: a gap in an existing skill, a correction, a new repeatable workflow worth encoding, or a decision about whether guidance belongs in one project or in the central skills repository.

## MyContextProtocol and MCP

Skills in this repository ([countablenewt/skills](https://github.com/countablenewt/skills)) are distributed through **MyContextProtocol** and exposed to agents as **MCP endpoints**. Treat every `SKILL.md` as a stable, consumer-facing contract:

- Write precise, testable steps and explicit **when not to use** guidance.
- Avoid secrets, one-off personal paths, and vague tool references.
- If a change narrows behavior or could break existing callers, call that out in the proposal (migration or compatibility note).

## When to use

- The user or agent notices an error, gap, or improvement in a skill from this repo or asks to suggest changes upstream.
- A conversation produces a workflow that could become a new skill.
- The user asks to contribute a skill or to decide where a skill should live.

## Placement: project vs central repository

**Keep the skill in the consumer project** (for example `.cursor/skills/<name>/` in that repo) when:

- It is tightly coupled to one codebase, stack, or deployment.
- It contains proprietary or client-specific context.
- It is experimental or likely to churn before it is worth sharing.

**Propose adding the skill to the central repository** when:

- It is reusable across projects without confidential details.
- Triggers and scope are clear enough for other agents to apply reliably.
- Publishing it as an MCP-backed workflow via MyContextProtocol would help others.

Filing the proposal is always in **Linear** (see below), not through a separate GitHub issue workflow.

## Workflow: Linear and the Skills team

**Linear is the system of record** for skill suggestions and new-skill proposals. Issues may **sync into this Git repository** for visibility; humans might see them on GitHub or in a clone. **Agents must create and update work in Linear only**—do not open or manage issues through GitHub as a parallel process.

1. Follow [linear-workflow/SKILL.md](../linear-workflow/SKILL.md): identify or create the right issue, confirm scope, set active status, and comment as the plan solidifies.
2. **Associate work with the Skills team** in Linear when creating or linking issues for this repository (use the team field, project, or labels your workspace uses for the Skills team—whichever applies).
3. **Suggestions to existing skills** — In the Linear issue or a comment, include:
   - Skill path (for example `linear-workflow/SKILL.md`).
   - Problem or gap.
   - Proposed change; optional patch-style wording.
4. **New skills** — In a Linear comment, include:
   - A full **draft `SKILL.md`**: YAML frontmatter (`name`, `description`) plus the markdown body.
   - One or two sentences on triggers, scope, and why it belongs in the central repo (if proposing central).
5. When implementation will touch git, note branch/PR expectations per team norms without bypassing linear-workflow.

## When you are already in this repository

Still use linear-workflow first. This skill does not replace it; it adds how to frame skill content for MCP exposure and how to route proposals through the **Skills** team in Linear.

## Other guidance

- For structure and quality of new skills (frontmatter, descriptions, layout), follow Cursor’s **create-skill** skill or your organization’s authoring conventions; do not paste entire third-party skill docs into Linear—link or summarize.
- Do not instruct anyone to install central skills by copying into `~/.cursor/skills-cursor/`; that directory is reserved for Cursor’s built-in skills.

## Outcome

After using this skill appropriately:

- Proposals for central skills or improvements are tracked in **Linear** under the **Skills** team, with enough detail to implement or review.
- Authors understand that merged skills here are consumed via **MyContextProtocol** as **MCP** and write accordingly.
- Project-local vs central placement is explicit.
