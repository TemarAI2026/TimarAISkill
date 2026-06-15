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

- **Skill definition (AI trigger layer):**
  - [`SKILL.md`](./SKILL.md) — Primary skill definition for AI assistants
- MCP-first public entry:
  - [`docs/mcp/README.md`](./docs/mcp/README.md)
  - [`docs/mcp/README.en.md`](./docs/mcp/README.en.md)
  - [`docs/mcp/README.zh-TW.md`](./docs/mcp/README.zh-TW.md)
- AI-facing guidance entry:
  - [`docs/skill/README.md`](./docs/skill/README.md)
  - [`docs/skill/README.en.md`](./docs/skill/README.en.md)
  - [`docs/skill/README.zh-TW.md`](./docs/skill/README.zh-TW.md)

## Runtime: TimarAIMCP (MCP Server)

This repository (`TimarAISkill`) is the **documentation & guidance layer**. The **runtime execution** is handled by the companion project:

### [TimarAIMCP](https://github.com/your-org/TimarAIMCP) — MCP Server

The MCP server that exposes Timar's payment, payout, and balance APIs as MCP tools. AI assistants invoke these tools through the Skill layer defined in this repository.

```
SKILL.md (this repo)     →    understand intent, select tool
         ↓ MCP protocol
TimarAIMCP (companion)   →    sign request, call API, return result
         ↓ HTTP
Timar Public API         →    execute business logic
```

## Directory structure

- `docs/mcp/`: MCP-first public overview and capability routing
- `docs/skill/hub/`: AI-facing routing and MCP capability entry documents
- `docs/skill/domains/`: shared rules and current published capability guides
- `docs/skill/references/`: exact headers, endpoints, models, statuses, and checklists
- `docs/skill/examples/`: runnable integration examples for supported languages
- `docs/specs/`: approved design documents for repository changes
- `docs/plans/`: implementation plans derived from approved designs

## Maintenance note

This repository should continue evolving toward an MCP-first public documentation model. Keep cross-language parity, keep exact facts source-backed, and avoid duplicating business logic concepts across `skill`, `x402`, or future protocol adapters.
