# MCP Public Docs

## Positioning

This directory is the public MCP documentation entrypoint for external integrators and AI coding assistants working with the Temar platform.

In the current architecture:

- `MCP` is the open capability base
- `skill` is the AI-facing guidance and trigger layer
- `x402` and future protocol surfaces are upper-layer adapters above MCP

If a future integration surface needs payment capabilities, payout capabilities, or shared signing behavior, it should route into MCP rather than re-create a separate business core.

## Who this is for

- external merchants or engineering teams integrating Temar open capabilities
- developers using Codex, Claude Code, Cursor, or similar AI coding assistants
- solution engineers who need a fast understanding of MCP capability boundaries and call paths

## What this directory assumes

These docs assume the following work is already complete before runtime integration begins:

- merchant registration and access enablement
- required business admission and internal review
- API credentials already issued

At runtime, the integration focus is narrower:

- choose the target environment
- configure `apiKey` and `secretKey`
- generate signing inputs
- call MCP capabilities
- persist returned identifiers and statuses correctly

## Fast start path

Use this reading order if you are integrating from scratch:

1. Read [`overview/architecture.en.md`](./overview/architecture.en.md) to understand what MCP owns and what it does not own.
2. Read [`overview/environments.en.md`](./overview/environments.en.md) to understand environment separation and runtime assumptions.
3. Read [`overview/auth-signing.en.md`](./overview/auth-signing.en.md) before generating any client code.
4. Pick the capability page you need:
   - [`capabilities/payment.en.md`](./capabilities/payment.en.md)
   - [`capabilities/payout.en.md`](./capabilities/payout.en.md)
   - [`capabilities/balance.en.md`](./capabilities/balance.en.md)
   - [`capabilities/notifications.en.md`](./capabilities/notifications.en.md)
5. Use [`references/common.en.md`](./references/common.en.md) together with the current exact reference pages for final contract validation.

## Phase-one capability scope

Current public MCP capability scope:

- payment
- payout
- balance
- notifications
- shared runtime calling conventions

Current phase-one non-scope:

- merchant registration workflows
- KYB or KYC process handling
- settlement or ledger internals
- wallet implementation internals
- internal reconciliation and operations procedures
- chain-level execution details outside the published response contract

## Five-step runtime checklist

When using MCP in production code or with an AI coding assistant, follow this order:

1. Confirm the target environment and credentials.
2. Confirm required headers and signing inputs.
3. Confirm the capability-specific request fields and endpoint path.
4. Persist the returned platform identifiers, status values, and trace data.
5. Implement safe follow-up handling for query, retry, cancellation, or notification flows.

## Common mistakes

- treating `docs/mcp/` as merchant onboarding docs instead of runtime integration docs
- inventing request fields that are not in the current public model
- mixing payment and payout semantics in one generic flow
- ignoring the platform `orderId` returned by create operations
- generating code from summaries without checking the exact reference facts

## Relationship to `docs/skill`

`docs/mcp/` is the MCP-first public entry layer.  
`docs/skill/` remains in place as the AI-facing guidance layer and the current exact published crypto reference layer.

In the current phase:

- `docs/mcp/` owns overall explanation, onboarding path, and capability navigation
- `docs/skill/references/` still holds the exact fact layer for the current published capability set

Use both layers together: start in `docs/mcp/`, then validate against the exact current references before implementation or release.
