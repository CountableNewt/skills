---
name: linear-workflow
description: Use Linear as the work-tracking source of truth before and during implementation. Trigger before coding, planning, or branch changes when work may map to a Linear issue.
---

# Linear Workflow

Use this skill whenever work should be tracked in Linear. The goal is simple: identify the right issue, move it into active work, and keep the issue current enough that another engineer can understand status, scope, blockers, and validation without reconstructing the session.

## When To Use

- Before any coding task, fix, implementation, or technical plan
- When starting work on a branch that may correspond to a tracked issue
- When the user references a Linear issue directly
- When scope changes, blockers appear, or work splits into follow-up issues

If the task is clearly untracked and no relevant issue exists, confirm that quickly and proceed. Do not force Linear usage where it does not apply, but always check first.

## Instructions

1. **Identify the current work item first.**
Check the current branch and the user request for a Linear identifier. If one is not explicit, search Linear using the branch name, task summary, or nearby issue wording until you either find the matching issue or determine that none exists.

2. **Confirm the issue before doing work.**
Open the issue and verify it is the correct scope. If the issue is ambiguous, stale, or clearly mismatched to the requested work, raise that early instead of silently proceeding under the wrong ticket.

3. **Move active work into the appropriate status.**
When you begin planning or implementation, set the issue to the team's active working state such as `In Progress`. Do this before making code changes unless the team intentionally uses a different workflow.

4. **Record the plan when it becomes concrete.**
If you develop a non-trivial approach, leave a concise comment on the issue describing the implementation plan, major decisions, or expected risks. The comment should help a reviewer or teammate understand what is about to happen.

5. **Keep Linear updated at decision points, not just at the end.**
Add comments when scope changes, blockers appear, assumptions are invalidated, work is handed off, or verification reveals unexpected behavior. Prefer short, meaningful updates over noisy status spam.

6. **Split newly discovered work deliberately.**
If you uncover additional work that should not be silently folded into the current issue, create or link a follow-up issue and note the relationship. Use a sub-issue only when the child work is truly part of the parent scope; otherwise create a separate related issue.

7. **Fill in metadata when you can do so confidently.**
If labels, priority, project, estimate, assignee, dependencies, or related issues are obvious from context, add or correct them. Do not guess at metadata that could misroute ownership or planning.

8. **Capture verification before closing your loop.**
When implementation is complete, leave a brief note summarizing what changed and how it was verified. Include the important checks you actually ran, especially if coverage is partial or a known risk remains.

9. **Let team policy determine final closure.**
If the team's workflow closes issues on merge, hand off by moving the issue to the appropriate review state rather than marking it `Done` yourself. If the team expects manual closure, follow that convention explicitly.

10. **If no issue exists, say that plainly.**
When work is intentionally untracked, or you cannot find a relevant issue after a reasonable check, proceed without fabricating one unless the user or team workflow requires issue creation.

## Operating Notes

- Prefer the Linear tools available in the current environment rather than hard-coding one integration path.
- Use branch names and commit/PR context as clues, not as the source of truth.
- Keep comments concise and decision-oriented. Linear should explain the work, not mirror every terminal action.
- Escalate early when the requested change and the issue scope do not match.

## Outcome

By the time you finish using this skill, the current task should have:

- A confirmed Linear issue, or an explicit determination that no issue applies
- An accurate active status while work is underway
- Useful comments for plan, scope changes, blockers, and verification
- Clean issue relationships and metadata when follow-up work is discovered

## Reference

See [AGENTS.md](../AGENTS.md) for the full workflow.
