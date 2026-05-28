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
