# Repository Map

## Purpose

This file explains where documentation belongs in `TimarAISkill` and how the repository is organized.

## Root files

- `README.md`: repository entrypoint and MCP-first positioning
- `CONTRIBUTING.md`: contributor rules and update expectations
- `CHANGELOG.md`: notable repository and documentation changes

## `docs/`

The `docs/` directory contains two kinds of content:

- MCP-first public documentation under `docs/mcp/`
- public AI-facing documentation under `docs/skill/`
- repository-maintenance support documents such as specs, plans, and workflow references

## `docs/mcp/`

`docs/mcp/` is the MCP-first public documentation home.

Its role is to provide:

- MCP overview
- environment guidance
- auth and signing guidance
- capability routing for payment, payout, balance, and notifications

It stays high-level in phase one and routes readers into current published fact and domain documents when exact contract detail is needed.

## `docs/skill/`

`docs/skill/` is the main public documentation tree for external integrators and AI coding assistants.

In the current architecture framing:

- MCP is the capability base
- `docs/skill/` is the AI-facing public guidance layer for using MCP-backed capabilities
- `skill`, `x402`, and future protocol surfaces are upper-layer adapters or trigger paths

Current language model:

- default `.md`: Simplified Chinese
- `.en.md`: English
- `.zh-TW.md`: Traditional Chinese

## `docs/skill` layers

### `hub/`

Use `hub/` for entry routing, reading order, and MCP capability navigation.

### `domains/`

Use `domains/` for task-oriented guidance and current published capability flows.

- `domains/shared/`: cross-capability rules
- `domains/crypto/`: current published crypto capability domain
- `domains/fiat/`: reserved future domain

### `references/`

Use `references/` for exact contract facts that must not be guessed.

Examples:

- headers
- endpoints
- request and response models
- statuses
- error codes
- integration checklists

### `examples/`

Use `examples/` for runnable or near-runnable samples that demonstrate how to use the documented contract correctly.

## `docs/specs/`

Use `docs/specs/` for approved design documents that explain what a repository change should accomplish before implementation.

## `docs/plans/`

Use `docs/plans/` for implementation plans derived from approved specs.

## Expansion direction

- `crypto` is the current published domain.
- `fiat` remains reserved for future release.
- future public domains should reuse the same `hub / domains / references / examples` structure
- future protocol adapters should route into MCP rather than define separate business cores

## Placement rule

When adding new content, decide first whether it is:

- public integration content for external readers
- repository-maintenance content for contributors

If it is public integration content, place it under `docs/skill/`. If it is process, design, planning, or repository-governance content, place it in the repository root or another appropriate `docs/` support location.
