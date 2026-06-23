# code-review

A senior, version-aware frontend code reviewer for React / Next.js / TypeScript / Tailwind. Reviews
your **code changes** and returns a narrative verdict with concrete fixes, references, and (when useful)
diagrams and metadata.

## What it does

The `code-review-agent` auto-detects your stack from config files and runs version-gated review phases —
security, architecture, hooks, class components, build tool, Next.js, TypeScript, styling, performance,
error handling, accessibility, testing, API/async, code quality, and governance. It reports findings
**only on the changed files** (reading surrounding code for context) and never flags pre-existing issues.

Two ways to scope the review:

| Mode | What gets reviewed | GitHub auth |
|------|--------------------|-------------|
| **Local changes** (default) | Your working-tree diff — tracked + untracked/new files | None |
| **PR link / number** | The changed files of that pull request | Optional (see below) |

## Installation

> **Important:** Run these commands in the **Claude CLI** terminal, not the IDE chat extension.

**Step 1 — Add this marketplace to your project:**

```
/plugin marketplace add git@github.com:simform-git/claude-plugins-react-mern.git
```

**Step 2 — Install the plugin:**

```
/plugin install code-review@simform-reactjs-plugins
```

**Step 3 — Reload plugins:**

```
/reload-plugins
```

## Usage

```
/review-changes [pr-link-or-number]
```

| Argument | Behavior |
|----------|----------|
| *(none)* | Reviews the **changed files in your working tree** — the everyday pre-PR check |
| PR link or number | Reviews that **pull request's** changed files (e.g. `.../pull/42` or `42`) |

## Setup & auth

**Local change review needs no setup or GitHub auth** — it uses plain git and works for everyone out of
the box.

**PR-link review** needs GitHub access, and it is **per-user and optional** — only for that feature.
When you give a PR link, the diff is resolved by the first available of a 3-tier fallback:

1. **GitHub MCP** — this plugin bundles GitHub's hosted remote MCP server (`github`). Authenticate it
   once with **`/mcp`** (OAuth — no Docker, no personal access token to manage). Run `/mcp` anytime to
   check or refresh its status.
2. **`gh` CLI** — if you'd rather not use MCP: `gh auth login`, then PRs are fetched via `gh pr diff`.
3. **Local git** — for a PR on the current repo, the agent can `git fetch` the PR ref directly (no GitHub auth).

The bundled MCP server stays **idle until you actually review a PR** — it never prompts during local
reviews, and users who never authenticate it can still use everything else without errors.

## Output

A narrative review with the **verdict at the top**, then Critical / Major / Minor findings (each with a
Suggested Solution + reference links; before/after code blocks for complex fixes), "What's Done Well",
Governance Findings, and Reviewer Notes. A Mermaid diagram is added when the change is structural enough
to benefit from one, and a compact metadata block is included for PR reviews or dashboard output.

## Part of

The `simform-reactjs-plugins` marketplace, alongside
[`react-component-generator`](../react-component-generator) and [`copilot-to-claude`](../copilot-to-claude).
