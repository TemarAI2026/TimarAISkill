# MCP Architecture

## MCP role

MCP is the unified open capability base for the Timar platform.

Its responsibilities are:

- providing a consistent open capability entrypoint
- unifying the runtime calling model
- unifying signing, request identity, error semantics, and capability routing

## Relationship to upper layers

- `skill`: the AI-facing guidance and trigger layer
- `x402`: a protocol adapter layer
- future protocols: also upper-layer adapters above MCP

These upper entry surfaces should not maintain separate business cores. They should route into MCP.

## What MCP owns in phase one

- payment capability path
- payout capability path
- balance capability path
- notification capability path
- common runtime calling conventions

## What MCP does not own in phase one

- merchant registration workflows
- the KYB/KYC process itself
- internal wallet implementation details
- settlement or ledger internals
- internal reconciliation and operations procedures

These remain either prerequisites or lower-layer platform concerns.
