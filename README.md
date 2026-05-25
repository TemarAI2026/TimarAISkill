# TimarAISkill

This repository is the standalone home for public AI-facing integration assets and future skill-related documentation.

## Current positioning

- `MCP` is the primary open capability base for runtime integration.
- `skill`, `x402`, and future payment protocols are upper-layer trigger or adapter surfaces above MCP.
- The current published public domain remains crypto-first, with fiat reserved for future expansion.

## What this repository is for

- Public integration guidance for external users and AI coding assistants
- MCP-backed capability guidance for payment, payout, balance, query, and notification usage
- Multilingual documentation in Simplified Chinese, English, and Traditional Chinese

## Main entrypoint

- [`docs/skill/README.md`](./docs/skill/README.md)
- [`docs/skill/README.en.md`](./docs/skill/README.en.md)
- [`docs/skill/README.zh-TW.md`](./docs/skill/README.zh-TW.md)

## Directory structure

- `docs/skill/hub/`: routing and MCP capability entry documents
- `docs/skill/domains/`: shared rules and current published capability guides
- `docs/skill/references/`: exact headers, endpoints, models, statuses, and checklists
- `docs/skill/examples/`: runnable integration examples for supported languages
- `docs/specs/`: approved design documents for repository changes
- `docs/plans/`: implementation plans derived from approved designs

## Maintenance note

This repository should continue evolving toward an MCP-first public documentation model. Keep cross-language parity, keep exact facts source-backed, and avoid duplicating business logic concepts across `skill`, `x402`, or future protocol adapters.
