# AGENTS.md

## Purpose

This repository uses a planner/executor workflow:

- ChatGPT acts as the planner, reviewer, and acceptance owner.
- Codex acts as the implementation executor.
- GitHub Issues, branches, pull requests, and CI are the source of truth.

Codex should treat the linked GitHub Issue as the task contract. If an Issue conflicts with this file, stop and report the conflict in the PR instead of guessing.

## Repository context

AirPodsDesktop is a Windows desktop application written primarily in C++20 with Qt 5.15.2 and CMake.

The existing Windows CI is the canonical build reference:

- Visual Studio 2019
- Win32 target
- CMake
- vcpkg
- Qt 5.15.2
- RelWithDebInfo

Read `Docs/Build.md`, `CMakeLists.txt`, and relevant existing source files before making architectural changes.

## Execution rules

1. Work on a dedicated branch. Never commit directly to `main`.
2. Keep scope limited to the linked Issue and its acceptance criteria.
3. Prefer the smallest change that fully satisfies the task.
4. Preserve existing public behavior unless the Issue explicitly requires a behavior change.
5. Do not perform unrelated refactors, dependency upgrades, formatting sweeps, or file moves.
6. Do not change licensing, attribution, release configuration, secrets, or repository ownership metadata unless the Issue explicitly requires it.
7. Do not weaken tests, CI, validation, error handling, or security checks to make a task pass.
8. Do not add telemetry, network calls, persistent data collection, or external services without explicit acceptance criteria.
9. Avoid introducing new dependencies unless they are necessary. Explain any new dependency in the PR.
10. Match the local coding style and surrounding patterns before introducing new abstractions.

## Required task intake

Before implementation, summarize in the PR description:

- the user-visible or developer-visible goal;
- the files/subsystems expected to change;
- any assumptions;
- the verification plan.

If the task is ambiguous in a way that can materially change behavior, do not guess. Mark the PR as draft and explain the blocking question.

## Verification

For code changes:

- Build or run the most relevant available tests.
- Use the existing Windows GitHub Actions workflow as the primary CI gate.
- If local verification is not possible, say exactly what was not run and why.
- Never report a test as passing unless it actually ran.

For documentation-only or workflow-template changes, verify file paths, links, and syntax and state that no product build was required.

## Pull request requirements

Every implementation PR should include:

- Linked Issue.
- Summary of the approach.
- Files or components changed.
- Tests/checks actually run and their results.
- Known limitations or risks.
- Screenshots for visible UI changes when practical.
- A clear statement if anything remains unverified.

Keep the PR draft until the implementation is complete enough for planner review.

## Review loop

The planner may request changes on a PR. Codex should:

1. Read every unresolved review comment.
2. Update the implementation only as needed to resolve those comments.
3. Re-run relevant verification after changes.
4. Reply with a concise summary of what changed.
5. Do not mark planner feedback resolved without addressing it.

## Definition of done

A task is done only when:

- all acceptance criteria in the Issue are satisfied;
- required CI/checks pass, or any unavoidable gap is explicitly documented;
- no unresolved planner review remains;
- no out-of-scope changes are included;
- the PR description accurately reflects the final implementation.

Do not merge the PR unless the planner or repository owner explicitly authorizes merging.
