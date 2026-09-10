# AIARAP-issues

Centralized, code-free issue tracker for all 4 AIARAP app repos. See `AIARAP---DOCUMENTATION/docs/agents/issue-tracker.md` for the full labeling/workflow convention this repo implements.

## Sandcastle dispatch (this repo's only automation)

- `.github/workflows/dispatch-to-repo.yml` — fires when an issue is labeled `Sandcastle` (or `retry:sandcastle`): checks its `repo:*` label and `Blocked by #N` references, and if unblocked, fires the target app repo's `agent-implement.yml`.
- `.github/workflows/promote-blocked.yml` — fires when any issue closes: re-checks every other open `Sandcastle`-labeled issue that names it as a blocker, and adds `retry:sandcastle` once all of that issue's blockers are closed.

## Required one-time setup (not yet done by this implementation — do this before dispatch will work)

1. **`AGENT_PAT` repo secret**, here and on all 4 app repos: a GitHub PAT (fine-grained, or classic `repo` + `workflow` scope) belonging to a bot/service account, with read/write access to this repo and all 4 app repos. Needed because `GITHUB_TOKEN`-driven label/comment writes across repos are not possible, and because a `GITHUB_TOKEN`-driven `workflow_dispatch` from this repo would be silently ignored by the target repo (GitHub's own restriction on `GITHUB_TOKEN`-triggered workflow runs).
2. **`CLAUDE_CODE_OAUTH_TOKEN` repo secret**, on all 4 app repos (not this one — this repo runs no agent).
3. Each app repo's default working branch for Sandcastle is `dev` (hardcoded in each `agent-implement.yml`) — matches the existing per-repo CI convention already in place.
