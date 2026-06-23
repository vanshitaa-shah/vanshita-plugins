# Review Changes

Review your code changes with the `code-review-agent` — a senior, version-aware frontend
reviewer for React/Next.js/TypeScript/Tailwind.

## Usage

`/review-changes [pr-link-or-number]`

- **No argument (default)** — review the **changed files in your working tree** (the everyday
  pre-PR check: "I changed 5 files, review them"). Needs no GitHub auth.
- **A PR link or number** (e.g. `https://github.com/org/repo/pull/42` or `42`) — review that
  pull request's changed files.

## Steps

1. **Resolve the changeset.**
   - **No argument:** get tracked changes with `git diff HEAD` and `git diff --stat HEAD`, and
     untracked/new files with `git status --porcelain` / `git ls-files --others --exclude-standard`.
   - **PR link/number:** resolve the PR's changed files and diff via the first available of this
     **3-tier fallback** (so no specific auth is mandatory):
     1. **GitHub MCP** (`github` server bundled with this plugin) — if authenticated. Richest:
        PR metadata, changed files, diff, and review comments.
     2. **`gh` CLI** — `gh pr diff <n>` / `gh pr view <n> --json files` if `gh` is installed and
        `gh auth login` is done.
     3. **Local git** — `git fetch origin pull/<n>/head:pr-<n>` then `git diff <base>...pr-<n>`
        (works for the current repo with no GitHub auth).
   - If there are no changes (or no PR source is reachable), report what's missing — e.g. authenticate
     via `/mcp` or run `gh auth login` — and stop. Do not break the default local flow.

2. **Delegate to the review agent.**
   - Invoke the **`code-review-agent`** with the resolved changed files as scope.
   - The agent reads the repo's config files to auto-detect the stack and reviews **only the
     changed files** (using surrounding code for context).

3. **Emit the agent's narrative review verbatim:** a one-line CONTEXT CONFIRMED stack summary,
   the **verdict at the top**, then Critical / Major / Minor findings (each with a Suggested Solution +
   reference links; complex fixes shown as before/after code blocks), "What's Done Well", Governance
   Findings, and Reviewer Notes. The agent adds a Mermaid diagram when the change is structural enough
   to benefit from one, and a compact metadata block for PR reviews (or dashboard output).

## Setup & auth

- **Local change review needs nothing** — it uses plain git and works for everyone out of the box.
- **PR-link review** needs GitHub access, and it's **per-user and optional** (only for this feature):
  - This plugin bundles GitHub's hosted remote MCP server (`github`). Authenticate it once with
    **`/mcp`** (OAuth — no Docker, no token to manage). Run `/mcp` anytime to check its status.
  - Or skip MCP and use the **`gh` CLI** (`gh auth login`), or the local-git fallback for the current repo.
- The bundled MCP server is idle until you actually review a PR — it never prompts during local reviews.

## Notes

- Scope is limited to the changed files — pre-existing issues in untouched code are out of scope.
- All suggestions are **version-gated**: the agent detects the React/Next.js/TypeScript/Tailwind
  versions before recommending any pattern.
