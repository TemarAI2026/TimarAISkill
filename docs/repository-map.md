# Repository Map

## Purpose

This file explains where documentation belongs in `TimarAISkill` and how the repository is organized.

## Root files

- `README.md`: repository entrypoint and high-level purpose
- `CONTRIBUTING.md`: contributor rules and update expectations
- `CHANGELOG.md`: notable repository and documentation changes

## `docs/`

The `docs/` directory contains two kinds of content:

- public AI-facing documentation under `docs/skill/`
- repository-maintenance support documents such as specs, plans, and workflow references

## `docs/skill/`

`docs/skill/` is the main public documentation tree for external integrators and AI coding assistants.

Current language model:

- default `.md`: Simplified Chinese
- `.en.md`: English
- `.zh-TW.md`: Traditional Chinese

## `docs/skill` layers

### `hub/`

Use `hub/` for entry routing, reading order, and domain navigation.

### `domains/`

Use `domains/` for task-oriented guidance and business-flow instructions.

- `domains/shared/`: cross-domain rules
- `domains/crypto/`: current published crypto domain
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
- Future public domains should reuse the same `hub / domains / references / examples` structure instead of introducing a new layout.

## Placement rule

When adding new content, decide first whether it is:

- public integration content for external readers
- repository-maintenance content for contributors

If it is public integration content, place it under `docs/skill/`. If it is process, design, planning, or repository-governance content, place it in the repository root or another appropriate `docs/` support location.
