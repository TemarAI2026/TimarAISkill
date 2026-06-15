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

## 🚀 Quick Start for AI Agents

**Any AI Agent that supports X402 protocol can use Timar payment capabilities in 3 steps:**

1. **Read the SKILL file** — understand available capabilities and endpoints
2. **Send HTTP requests** to the X402 adapter endpoint
3. **Pay with USDC** (automatically via X402) → get business results

No registration. No API key. No deployment.

## Main entrypoint

**For AI Agents (recommended starting point):**

| Language | SKILL File |
|----------|-----------|
| 简体中文 | [`SKILL.md`](./SKILL.md) |
| 繁體中文 | [`SKILL.zh-TW.md`](./SKILL.zh-TW.md) |
| English | [`SKILL.en.md`](./SKILL.en.md) |

**For developers building integrations:**

- MCP-first public entry:
  - [`docs/mcp/README.md`](./docs/mcp/README.md)
  - [`docs/mcp/README.en.md`](./docs/mcp/README.en.md)
  - [`docs/mcp/README.zh-TW.md`](./docs/mcp/README.zh-TW.md)
- AI-facing guidance entry:
  - [`docs/skill/README.md`](./docs/skill/README.md)
  - [`docs/skill/README.en.md`](./docs/skill/README.en.md)
  - [`docs/skill/README.zh-TW.md`](./docs/skill/README.zh-TW.md)

## How It Works

```
SKILL.md (this repo)
    ↓ AI Agent reads, understands capabilities
Any AI Agent
    ↓ HTTP + USDC payment (X402 protocol)
X402 Adapter (TimarAIMCP)
    ↓ MCP tool invocation + HMAC sign
Timar Public API
    ↓ executes business logic
Result returned to Agent
```

**The SKILL file is the single source of truth for AI Agents.** It defines what capabilities exist, how to call them, and how to interpret results — regardless of which Agent framework is used.

## Runtime: TimarAIMCP (MCP Server + X402 Adapter)

This repository (`TimarAISkill`) is the **skill & documentation layer**. The **runtime execution** is handled by the companion project:

### [TimarAIMCP](https://github.com/your-org/TimarAIMCP) — MCP Server + X402 Adapter

Two entry modes:

| Mode | Transport | Auth | Best For |
|------|-----------|------|----------|
| **X402 Adapter** (`npm run start:x402`) | HTTP | USDC per call | Any AI Agent, pay-per-use |
| **MCP STDIO** (`npm start`) | STDIO | API Key + Secret | Local AI assistants (Claude Desktop, Cursor) |

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
