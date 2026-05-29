# CLAUDE.md — wild-rails-ai-ops

This file provides guidance to Claude Code when working in this repository.

## Identity

- **Repo:** `wild-rails-ai-ops`
- **Role:** Umbrella repo for the wild ecosystem (10 Ruby gems for running AI agents inside Rails apps under capability control)
- **Status:** Ecosystem-level documentation, dependency map, per-repo status table. No application code lives here.

## What this repo contains

- `README.md` — public-facing landing page for the wild ecosystem
- `ARCHITECTURE.md` — runtime layers and cross-gem dependency map
- `CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md` — standard governance
- `LICENSE` — Intent Solutions Proprietary

## What this repo does NOT contain

- No Ruby code. The 10 `wild-*` gems are independent repos at `jeremylongshore/wild-*`.
- No tests. (No code to test.)
- No CI workflows beyond markdown lint and link check.

## When to edit

- A `wild-*` gem changes status, version, or mission → update the per-repo status table in `README.md` (mirrored from the source-of-truth table in the workspace-level `wild/CLAUDE.md` §11).
- A new cross-gem dependency lands → update `ARCHITECTURE.md`.
- The Intent Solutions org profile changes how it features the ecosystem → update `README.md` accordingly.

## Single source of truth

The per-repo status table in `wild/CLAUDE.md` §11 (in the local workspace, not this repo) is authoritative. This repo's `README.md` mirrors that table. When they drift, update the workspace CLAUDE.md first, then sync this README.

## Truth & Presentation Audit

The 2026-05-28 truth audit is filed at `wild/000-docs/009-AT-AUDT-wild-ecosystem-truth-audit-2026-05-28.md` (in the workspace, not this repo). Cross-repo findings from the 10 per-repo appaudits are summarized in `ARCHITECTURE.md` § "Known v1 → v2 adoption gaps".

## Out of scope here

- Per-gem implementation details — those live in each `wild-*` repo's own `000-docs/`
- Per-gem CLAUDE.md content — each `wild-*` repo has its own
- Beads — this repo may host ecosystem-wide epic beads (Phase F-pending decision); per-gem beads live in each gem's `.beads/`


<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:7510c1e2 -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

**Architecture in one line:** issues live in a local Dolt DB; sync uses `refs/dolt/data` on your git remote; `.beads/issues.jsonl` is a passive export. See https://github.com/gastownhall/beads/blob/main/docs/SYNC_CONCEPTS.md for details and anti-patterns.

## Session Completion

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
<!-- END BEADS INTEGRATION -->
