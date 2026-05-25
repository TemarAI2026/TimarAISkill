# MCP Public Docs

## Positioning

This directory is the public MCP documentation entrypoint for external integrators and AI coding assistants working with the Timar platform.

In the current architecture:

- `MCP` is the open capability base
- `skill` is the AI-facing guidance and trigger layer
- `x402` and future protocol surfaces are upper-layer adapters above MCP

## Who this is for

- External merchants or engineering teams integrating Timar open capabilities
- Developers using Claude Code, Cursor, Codex, or similar AI coding assistants
- Solution engineers who need a fast understanding of MCP capability boundaries and call paths

## Runtime prerequisites

This directory assumes the following prerequisites are already complete:

- merchant registration and access enablement
- required business admission and internal review
- API credentials already issued

At runtime, the focus is not on onboarding. The focus is on:

- choosing the environment
- configuring `apiKey` and `secretKey`
- generating signing inputs
- invoking MCP capabilities

## Reading order

1. Start with [`overview/architecture.en.md`](./overview/architecture.en.md)
2. Continue with [`overview/environments.en.md`](./overview/environments.en.md)
3. Then read [`overview/auth-signing.en.md`](./overview/auth-signing.en.md)
4. Then enter the capability pages:
   - [`capabilities/payment.en.md`](./capabilities/payment.en.md)
   - [`capabilities/payout.en.md`](./capabilities/payout.en.md)
   - [`capabilities/balance.en.md`](./capabilities/balance.en.md)
   - [`capabilities/notifications.en.md`](./capabilities/notifications.en.md)
5. Finally use [`references/common.en.md`](./references/common.en.md) together with the existing `docs/skill/references/` material for exact contract validation

## Relationship to `docs/skill`

`docs/mcp/` is the MCP-first public entry layer.  
`docs/skill/` remains in place as the AI-facing guidance layer and the current published crypto capability guidance layer.

In phase one:

- `docs/mcp/` owns overall explanation and capability routing
- `docs/skill/references/` continues to hold the exact fact layer for the current published capability set
