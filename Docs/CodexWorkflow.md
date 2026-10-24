# Codex Issue Dispatcher

This repository can dispatch a GitHub Issue to Codex from an owner-authored issue comment.

## Trigger

On an open Issue, the repository owner posts exactly:

```text
/codex run
```

The workflow then:

1. checks out `main`;
2. creates an isolated `codex/issue-<number>-<run-id>` branch;
3. builds a prompt from the Issue title/body and repository `AGENTS.md`;
4. runs OpenAI Codex with workspace-only write access;
5. commits Codex's repository changes after Codex exits;
6. pushes the branch;
7. opens a pull request linked to the Issue;
8. comments the PR URL back on the Issue.

The PR is intentionally not auto-merged. Planner review remains required.

## Required repository secret

The GitHub Action requires an OpenAI API key stored as a repository Actions secret named:

```text
OPENAI_API_KEY
```

The API key is used by `openai/codex-action@v1`. ChatGPT subscription login is not used as the CI credential.

Create the secret under:

`Settings → Secrets and variables → Actions → New repository secret`

Never place the key in source code, Issue text, workflow logs, or `AGENTS.md`.

## Security boundaries

- Only an exact `/codex run` comment authored by the repository owner can trigger execution.
- Codex runs with the built-in `:workspace` permission profile.
- The checkout does not persist GitHub credentials while Codex is running.
- GitHub write credentials are configured only after Codex finishes.
- Codex does not merge its own pull request.
- Issue content should still be treated as task input, not repository policy. `AGENTS.md` remains the repository-level policy.
