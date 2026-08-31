---
name: notion-project-documentation
description: Route project documentation into Notion subpages under the project page; use for docs, wiki pages, specs, ADRs, runbooks, guides, FAQs, notes, and knowledge capture that should live in Notion instead of local files.
---

# Notion Project Documentation

Use this skill when the user wants to create, update, or store project documentation. The default rule is that project documentation belongs in Notion as subpages under the project's Notion page, created or updated through an available Notion capability rather than local docs files.

## Quick start
1. Determine whether the request is documentation work: spec, wiki page, runbook, ADR, guide, FAQ, notes, decision log, project documentation, or knowledge capture.
2. Resolve the project parent page in Notion.
3. If the parent page is not explicit and you cannot identify one clear match, stop and ask the user which Notion page to use.
4. Search under that project context for an existing matching document before creating a new one.
5. Update the existing subpage if it already covers the requested topic; otherwise create a new subpage under the project page.

## Enforcement

- Treat Notion as the authoritative store for project documentation.
- Do not complete documentation tasks as local-doc-only work unless the user explicitly redirects scope away from Notion.
- If the parent project page is ambiguous, missing, or unconfirmed, block and ask instead of guessing.
- Do not silently create a new top-level project page in Notion. This skill only creates or updates subpages beneath an existing project page.

## Workflow

### 0) If a required Notion capability is unavailable, stop truthfully

Ask the user to select or connect a provider that can search, read, create, and update Notion pages. Do not claim a connection was configured, or prescribe a client-specific setup command, unless the active agent exposes and performs that capability.

### 1) Decide whether this skill applies
Use this skill when the user is asking to write or store documentation that should remain discoverable for a project or team. Common triggers:

- docs, documentation, wiki, project page
- spec, PRD, design doc, technical design
- ADR, decision record, architecture note
- runbook, playbook, guide, SOP, how-to
- FAQ, notes, meeting notes, knowledge capture

If the task is purely code, configuration, or an ephemeral draft with no documentation intent, this skill does not control the destination.

### 2) Resolve the parent project page
Resolve the Notion parent page in this order:

1. If the user supplied a Notion URL or page ID, use that page after a quick fetch to confirm it is the intended project page.
2. Otherwise, search Notion using the repo name, current branch, issue context, project name, and the user's wording.
3. If exactly one credible project page matches, use it.
4. If multiple plausible pages match, or no credible page is found, ask the user which parent page to use and stop there until they answer.

Do not invent a parent page. The blocking question should be direct, for example: "Which Notion project page should I store this under? Paste the page URL or page ID."

### 3) Check for an existing documentation subpage
Before creating anything new:

- Search Notion for an existing subpage under or closely related to the resolved project page.
- Use the requested title, topic keywords, and likely document type in the search.
- Fetch the best existing candidates before deciding whether to update or create.

Prefer updating an existing document when the new request is clearly a revision, continuation, or deeper version of the same topic.

### 4) Create or update the documentation page
- Use the bound Notion page-create capability with the resolved project page as the parent when creating a new document.
- Use the bound Notion page-update capability when updating an existing page.
- Structure the page so it is useful to future readers: summary, context, key decisions, steps, links, owners, dates, or open questions as appropriate to the document type.
- When useful, include backlinks or mentions to related project pages, specs, tasks, or decision records.

Do not mirror the same documentation into local markdown files unless the user explicitly asks for a local artifact in addition to Notion.

### 5) Report the outcome clearly
When finished, tell the user:

- which project page you used
- whether you created a new subpage or updated an existing one
- the title of the Notion page
- any follow-up gaps that still need user input

## Operating notes

- Prefer Notion MCP search, fetch, create, and update flows over ad hoc assumptions.
- Use the repo name and issue context as search clues, not proof.
- Be conservative about duplicate pages. Search and fetch first.
- If the user explicitly asks for both a Notion page and a local file, create the Notion page first unless they direct otherwise.
