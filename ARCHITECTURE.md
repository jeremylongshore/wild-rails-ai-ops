# Architecture — wild-rails-ai-ops

This document describes how the 10 `wild-*` gems fit together at runtime, the dependency graph between them, and the boundaries each one is responsible for.

For per-gem deep dives (entry points, file:line citations, failure modes, operator playbooks) see each repo's `000-docs/NNN-AT-AUDT-appaudit-2026-05-28.md`.

## Runtime layers

The ecosystem has four operational layers. Each gem belongs to exactly one.

### Layer 1 — Product-facing runtime (Archetype A)

What an AI agent actually talks to during a live session.

- **wild-rails-safe-introspection-mcp** — MCP server exposing 3 read-only Rails introspection tools (model reflection, schema inspection, bounded record read). Policy-enforced, audited, bounded. No writes, no console access, no admin.
- **wild-admin-tools-mcp** — MCP server exposing administrative actions (the inverse of the introspection gem — this is the write path). Every tool call is gated by `wild-capability-gate`.

### Layer 2 — Coordination & registry (Archetype D)

Cross-gem coordination primitives consumed by Layer 1 gems.

- **wild-capability-gate** — The decision point for "is this agent allowed to invoke this tool right now?" Consumed in-process by `wild-admin-tools-mcp`. Emits audit decisions to a configured sink.
- **wild-skillops-registry** — Skill advertisement and lookup across agents. v0.1.0 — currently standalone. Consumer adoption (likely by `wild-capability-gate` and the *-mcp gems) is planned for v2.

### Layer 3 — Telemetry & analytics (Archetype B)

Out-of-band data collection, processing, and analysis. Runs alongside Layer 1, not in the request path.

- **wild-session-telemetry** — Collects session events (calls, decisions, outcomes) with privacy-aware redaction. Emits to a configured sink.
- **wild-transcript-pipeline** — Ingests, normalizes, and processes raw transcripts (likely from telemetry export).
- **wild-gap-miner** — Consumes from `wild-session-telemetry` and `wild-transcript-pipeline` outputs. Classifies gaps: actions an agent could have taken but didn't, attempts that failed, holes in the capability model.

### Layer 4 — SDLC companion (Archetype C)

Engineer-facing, not runtime. Run during development, CI, or pre-deploy.

- **wild-permission-analyzer** — Static analysis of a host Rails app's permission model. Run before agents are deployed.
- **wild-test-flake-forensics** — Detects and triages flaky tests. Integrates with CI.
- **wild-hook-ops** — Generalizes hook lifecycle patterns that today live as ad-hoc code in `wild-admin-tools-mcp` and `wild-rails-safe-introspection-mcp`. Extracted in v1; consumer adoption planned for v2.

## Cross-layer dependencies

The only hard runtime dependency between Layer 1 gems and Layer 2 is `wild-admin-tools-mcp → wild-capability-gate` (declared in admin-tools' Gemfile). All other inter-gem relationships are data-flow (Layer 3) or planned-for-v2 (hook-ops, skillops-registry adoption).

```
admin-tools-mcp ────► capability-gate     (hard runtime dep, v1)
session-telemetry ──► transcript-pipeline ──► gap-miner   (data flow, v1)
*-mcp gems ──[v2]──► hook-ops              (planned)
*-mcp + capability-gate ──[v2]──► skillops-registry  (planned)
```

## Known v1 → v2 adoption gaps

These are documented in the 2026-05-28 truth audit and tracked per-repo:

1. **hook-ops not yet consumed** — `wild-admin-tools-mcp` and `wild-rails-safe-introspection-mcp` still use their own ad-hoc HookEmitter classes. Migration to `wild-hook-ops` is the v2 target. See each consumer's `000-docs/NNN-AT-AUDT-appaudit-2026-05-28.md`.
2. **skillops-registry standalone** — No other gem currently registers with it. v2 consumer adoption is the open question; see `wild-skillops-registry/000-docs/007-AT-AUDT-appaudit-2026-05-28.md`.
3. **Data contract symmetry** — Telemetry → transcript-pipeline → gap-miner contracts are documented on the consumer side; producer-side documentation completeness is being verified in the truth-audit cycle.

## Operating principles

These apply to every gem in the ecosystem:

- **Safety-first defaults** — Every privileged action is gated; every gate decision is audited.
- **Read-only-by-default** — Write paths are explicitly opt-in and require capability-gate approval.
- **Privacy-aware** — Telemetry redacts PII by default; opt-in for richer collection.
- **Auditable** — Decisions and actions are recorded with enough context to reconstruct what happened.
- **Composable** — Each gem has a single responsibility and a stable public surface.

## License

All gems and this umbrella repo are Intent Solutions Proprietary.
