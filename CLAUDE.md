# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

See [AGENTS.md](AGENTS.md) for the full development rules. Key rules are summarized here.

## Commands

```bash
npm install          # Install all dependencies
npm run build        # Build all packages (tui → ai → agent → coding-agent → mom → web-ui → pods)
npm run check        # Lint, format, and type check (run after every code change; must show full output)
./test.sh            # Run all tests (unsets LLM API keys)
./pi-test.sh         # Run pi coding agent from source (must run from repo root)
```

> `npm run check` requires a prior `npm run build` because web-ui needs compiled `.d.ts` files.

**NEVER run:** `npm run dev`, `npm run build`, or `npm test` directly. Use `./test.sh` for tests.

**Run a single test** (from the package root, not repo root):
```bash
npx tsx ../../node_modules/vitest/dist/cli.js --run test/specific.test.ts
```

**TUI package** uses Node's built-in test runner:
```bash
node --test --import tsx test/*.test.ts
```

## Architecture

npm workspaces monorepo. All packages under `packages/` share lockstep versioning. TypeScript, compiled with `tsgo`, formatted/linted with Biome (tabs, 120-char lines).

**Package dependency chain:**
```
pi-tui (standalone)
pi-ai (standalone)
  └─ pi-agent-core
       ├─ pi-coding-agent (also depends on pi-tui)
       ├─ pi-mom (also depends on pi-coding-agent)
       └─ pi-pods
pi-web-ui (depends on pi-ai, pi-tui)
```

**Package purposes:**
- `packages/ai` — Unified multi-provider LLM streaming API (OpenAI, Anthropic, Google, Bedrock, etc.). Handles tool calling, thinking, images, token/cost tracking, and cross-provider handoffs.
- `packages/agent` — Stateful agent runtime: tool execution, event streaming, state management.
- `packages/coding-agent` — Interactive CLI coding agent with read/write/edit/bash tools, sessions, extensions, skills, themes.
- `packages/tui` — Terminal UI library with differential rendering and markdown support.
- `packages/web-ui` — Web components for AI chat UIs (Lit, Tailwind CSS 4).
- `packages/mom` — Slack bot that delegates to the coding agent via Socket Mode.
- `packages/pods` — CLI for managing vLLM deployments on GPU pods.

**Root tsconfig.json** defines path aliases for all packages so they resolve to source (`packages/*/src/index.ts`) during development.

## Code Quality Rules

- No `any` types unless absolutely necessary. Check `node_modules` for type definitions rather than guessing.
- **Never use inline/dynamic imports** — no `await import()`, no `import("pkg").Type` in type positions. Always use top-level imports.
- Never remove or downgrade code to fix type errors from outdated dependencies; upgrade the dependency.
- All keybindings must be configurable — never hardcode key checks. Add defaults to `DEFAULT_EDITOR_KEYBINDINGS` or `DEFAULT_APP_KEYBINDINGS`.
- Ask before removing functionality that appears intentional.

## Git Rules (Critical for Parallel Agents)

Multiple agents may work simultaneously in the same worktree.

- **Only commit files YOU changed in YOUR session.**
- **Never use** `git add -A` or `git add .` — always `git add <specific-file-paths>`.
- **Never use:** `git reset --hard`, `git checkout .`, `git clean -fd`, `git stash`, `git commit --no-verify`.
- Include `fixes #<number>` or `closes #<number>` in commit messages when a related issue exists.
- On rebase conflicts: resolve only in your files; if the conflict is in a file you didn't touch, abort and ask the user.

## Changelog

Each package has `packages/*/CHANGELOG.md`. New entries always go under `## [Unreleased]`. Read the full `[Unreleased]` section before adding to avoid duplicate subsections. Never modify already-released version sections.

Subsection order: `### Breaking Changes`, `### Added`, `### Changed`, `### Fixed`, `### Removed`.

Attribution format:
- Internal (issue): `Fixed foo ([#123](https://github.com/badlogic/pi-mono/issues/123))`
- External (PR): `Added X ([#456](https://github.com/badlogic/pi-mono/pull/456) by [@user](https://github.com/user))`

## Releasing

Lockstep versioning — all packages share one version. `patch` for fixes/features, `minor` for API breaking changes (no major releases).

```bash
npm run release:patch   # or :minor
```

The script handles version bump, CHANGELOG finalization, commit, tag, publish, and adding new `[Unreleased]` sections.

## GitHub Issues & PRs

```bash
gh issue view <number> --json title,body,comments,labels,state
```

Add `pkg:*` labels to issues/PRs: `pkg:agent`, `pkg:ai`, `pkg:coding-agent`, `pkg:mom`, `pkg:pods`, `pkg:tui`, `pkg:web-ui`.

Never open PRs. Work in feature branches, then merge into main and push.

## Testing pi's TUI with tmux

```bash
tmux new-session -d -s pi-test -x 80 -y 24
tmux send-keys -t pi-test "cd /path/to/pi-mono && ./pi-test.sh" Enter
sleep 3 && tmux capture-pane -t pi-test -p
tmux send-keys -t pi-test "your prompt" Enter
tmux kill-session -t pi-test
```
