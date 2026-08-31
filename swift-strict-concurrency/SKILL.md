---
name: swift-strict-concurrency
description: Use for Swift 6 mode, strict concurrency, concurrency diagnostics, and warnings-as-errors workflows in Vapor/server-side Swift and iOS/Xcode Swift projects; enforce no new warnings and fix or track any warnings surfaced by the commands you run.
---

# Swift Strict Concurrency

Use this skill when work touches Swift code that is compiled with Swift 6 mode, strict concurrency checks, or a CI policy that treats warnings as errors. This skill is intentionally strict: agents must not introduce warnings, must treat surfaced warnings as in scope, and must prefer real concurrency-safe fixes over suppressions.

## When To Use

- When working in Swift 6 mode or in a project that has strict concurrency enabled
- When the build, test, or CI workflow treats warnings as errors
- When the user mentions strict concurrency, `Sendable`, actor isolation, `@MainActor`, or concurrency diagnostics
- When editing Vapor or other server-side Swift code that crosses request, task, or service boundaries
- When editing iOS or Xcode Swift code where UI, view models, services, or async flows may cross actor boundaries
- When running `swift build`, `swift test`, or `xcodebuild` could surface concurrency warnings that the task must keep clean

If the task does not involve Swift code, compiler diagnostics, or concurrency-related behavior, this skill is usually not needed.

## Core Rules

1. **Do not introduce warnings.**
Treat compiler warnings as blockers for the task at hand. A change is not complete if it leaves behind new warnings.

2. **Do not weaken warnings-as-errors policy.**
Do not remove `-warnings-as-errors`, relax project settings, or otherwise lower diagnostic strictness just to get a build through. Preserve or strengthen the existing warning policy.

3. **Any warning surfaced by your commands is in scope.**
If `swift build`, `swift test`, `xcodebuild`, or another command you run surfaces a warning, treat it as part of the current task. Do not ignore it because it predated your code path or because the original request seemed narrower.

4. **Fix warnings inline when it is safe and local.**
Prefer resolving surfaced diagnostics as part of the current change when the fix is clear, low risk, and consistent with the task.

5. **Track warnings that cannot be fixed safely inline.**
If a surfaced warning cannot be handled cleanly within the current task, create or update a follow-up item in the project's primary tracker and report that explicitly. Prefer Linear when the project uses it; otherwise use the repository's normal issue tracker. Activate the available issue-tracking workflow instead of depending on a sibling package path.

6. **Prefer real concurrency correctness over escape hatches.**
Favor actor isolation, `Sendable` conformance, immutable state, and clear async ownership boundaries. Do not reach first for `@unchecked Sendable`, `nonisolated(unsafe)`, blanket `@preconcurrency`, or suppression-style workarounds.

7. **Make shared state explicit and safe.**
Assume mutable shared state is a bug risk until proven otherwise. Protect it with actors, isolation, or a redesign that removes unsafe cross-task access.

8. **Fix the boundary, not just the symptom.**
When a warning points to a non-sendable capture, cross-actor mutation, or incorrect isolation, repair the underlying boundary instead of merely reshaping code until the warning disappears.

## Required Workflow

1. **Confirm the project's diagnostic posture first.**
Check whether the project is using Swift 6 mode, strict concurrency settings, or warnings-as-errors in package manifests, Xcode settings, CI scripts, or build commands. Treat the strictest active path as the baseline you must preserve.

2. **Run the normal verification command for the task.**
Use the project's standard build or test command such as `swift build`, `swift test`, or `xcodebuild`. If that command surfaces warnings, they are part of the task and must be addressed.

3. **Classify each surfaced warning by boundary type.**
Determine whether the warning comes from actor isolation, `Sendable` conformance, closure captures, mutable shared state, main-actor UI access, or task handoff across request or service boundaries. Fix the architectural cause before making cosmetic changes.

4. **Apply platform-appropriate fixes.**
For Vapor and server-side Swift:
- Prefer `Sendable` service and dependency boundaries.
- Avoid leaking non-sendable request-scoped or mutable state into detached or concurrent tasks.
- Do not hide server concurrency warnings just to satisfy CI.

For iOS and Xcode Swift projects:
- Keep UI-facing code correctly isolated, usually with `@MainActor` where ownership is genuinely main-thread-bound.
- Avoid passing mutable state or non-sendable closures across actor boundaries.
- Fix concurrency warnings in view models, controllers, coordinators, and supporting services instead of silencing them at the call site.

5. **Escalate deliberately when the warning is larger than the task.**
If a surfaced warning reveals broader architectural work that is unsafe to fold into the current change, record a follow-up in the project's tracker with enough detail to reproduce the warning and explain why it was not handled inline.

6. **Re-run verification until the output is clean.**
Finish by re-running the relevant build or test command and confirm there are no warnings in the surfaced output for the path you exercised.

## Verification Checklist

- Confirm the final build or test path you ran completed without warnings
- Confirm you did not relax warnings-as-errors or strict concurrency settings
- Confirm surfaced warnings were either fixed inline or tracked explicitly in the project's primary issue system
- Confirm actor isolation and `Sendable` changes reflect real ownership and are not cosmetic suppressions
- Confirm shared mutable state is not crossing task or actor boundaries unsafely
- Confirm platform-specific boundaries still make sense after the fix, especially request/task boundaries in Vapor and UI/main-actor boundaries in iOS code

## Common Anti-Patterns

- Adding `@unchecked Sendable` without proving the type is actually safe to share
- Using `nonisolated(unsafe)` to silence a warning instead of correcting ownership
- Blanket suppression or compatibility annotations that hide current problems without fixing them
- Capturing mutable state into concurrent tasks because it "works in practice"
- Relaxing CI or local build settings to avoid warning cleanup
- Treating warnings surfaced by your own verification commands as out of scope
- Marking entire subsystems `@MainActor` or otherwise over-isolating code when the real problem is narrower and should be fixed at the boundary

## Outcome

By the time you finish using this skill, the project should have:

- No warnings introduced by the change
- No unresolved warnings in the build or test path you exercised
- Concurrency fixes that improve real safety rather than merely suppressing diagnostics
- Follow-up tracking for any surfaced warning that could not be resolved safely within the task
