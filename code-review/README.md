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
/plugin marketplace add vanshitaa-shah/vanshita-plugins
```

**Step 2 — Install the plugin:**

```
/plugin install code-review@vanshita-plugins
```

**Step 3 — Reload plugins:**

```
/reload-plugins
```

## Usage

```
/review-changes [pr-link-or-number]
```

| Argument | Behavior | GitHub auth |
|----------|----------|-------------|
| *(none)* | Reviews the **changed files in your working tree** — the everyday pre-PR check | None |
| PR link or number | Reviews that **pull request's** changed files (e.g. `.../pull/42` or `42`) | Token / `gh` |

### A — Review your local changeset (no setup)

Make some edits in a React/Next.js repo (no need to commit), then:

```
/review-changes
```

Example prompts:

```
/review-changes
review my current changes
```

This uses plain `git diff` — works for everyone with zero GitHub auth.

### B — Review a Pull Request (needs a GitHub token, see setup below)

```
/review-changes review this PR https://github.com/owner/repo/pull/42
```

Example prompt:

```
review this pull request: https://github.com/owner/repo/pull/42
```

## Setup & auth

**Local change review needs no setup or GitHub auth** — it uses plain git and works out of the box.
**Only PR-link review needs GitHub access.** Set it up once with a Personal Access Token:

### Step 1 — Create a GitHub token

Pick the token type based on **who owns the repo**:

- **Repo you own / your org's repo** → a **fine-grained token** works
  (GitHub → Settings → Developer settings → *Fine-grained tokens*). Grant the repo
  **Pull requests: Read** + **Contents: Read**.
- **Repo you DON'T own** (you're just a collaborator/reviewer) → you **must use a
  classic token** (GitHub → Settings → Developer settings → *Tokens (classic)*) with the
  **`repo`** scope. Fine-grained tokens cannot target a repo owned by another user.
  - If the org uses SSO, click **Configure SSO → Authorize** on the token.

> Read-only access is enough — this reviewer never writes to your repo or comments on PRs.

### Step 2 — Store the token as an env var (in `~/.bashrc`)

The plugin's MCP server reads your token from the `GITHUB_PAT` environment variable. Add it to your
shell profile so Claude Code inherits it:

```bash
echo 'export GITHUB_PAT="ghp_your_token_here"' >> ~/.bashrc
source ~/.bashrc
```

Verify it's set:

```bash
echo $GITHUB_PAT
```

Then **restart Claude Code** so it picks up the variable.

### Step 3 — Confirm the MCP server is connected

```
/mcp
```

The `code-review:github` server should show **connected** (the token in the header authenticates it — no
OAuth/`/mcp authenticate` step needed).

### Fallbacks (if you'd rather not set a token)

The PR diff is resolved by the first available of a 3-tier fallback, so a token is not strictly mandatory:

1. **GitHub MCP** — the bundled `github` server, authenticated via the `GITHUB_PAT` token above. (Richest.)
2. **`gh` CLI** — run `gh auth login` once; PRs are then fetched via `gh pr diff`. **Best option for repos
   you don't own**, since it uses your account's own collaborator access — no token scoping to figure out.
3. **Local git** — for a PR on the current repo, the agent can `git fetch` the PR ref directly (no auth).

The bundled MCP server stays **idle until you actually review a PR** — it never prompts during local
reviews, and users who never set it up can still use everything else without errors.

## Output

A narrative review with the **verdict at the top**, then Critical / Major / Minor findings (each with a
Suggested Solution + reference links; before/after code blocks for complex fixes), "What's Done Well",
Governance Findings, and Reviewer Notes. A Mermaid diagram is added when the change is structural enough
to benefit from one, and a compact metadata block is included for PR reviews or dashboard output.

## Part of

The `vanshita-plugins` marketplace, alongside [`react-toolkit`](../react-toolkit).
