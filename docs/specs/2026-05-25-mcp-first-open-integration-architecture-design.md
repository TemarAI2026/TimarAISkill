# MCP-First Open Integration Architecture Design

## Summary

This design defines the next architecture direction for `TemarAISkill`: use MCP as the single open-integration capability base, then layer `skill`, `x402`, and future payment protocol adapters on top of it.

The goal is not to make `skill` the business core. The goal is to make MCP the only maintained open capability layer for runtime integration, while `skill`, `x402`, and future protocol surfaces become entry adapters that trigger or route into MCP.

This keeps the integration model maintainable, prevents duplicated business logic across protocols, and makes the documentation easier for AI coding assistants and external integrators to follow.

## Goals

- Establish MCP as the primary open-integration capability layer.
- Position `skill` as an AI-facing guidance and trigger layer, not the business execution layer.
- Position `x402` and future payment protocols as adapter layers above MCP.
- Keep runtime integration simple: choose environment, configure credentials and signing inputs, then call supported MCP capabilities.
- Avoid maintaining separate business logic stacks for `skill`, `x402`, and future protocols.

## Non-Goals

- This phase does not attempt to model internal merchant registration workflows in MCP runtime flows.
- This phase does not pull internal settlement, ledger, or wallet implementation details into public MCP design.
- This phase does not require every internal platform capability to be exposed through MCP immediately.
- This phase does not redesign existing crypto public reference facts unless architecture naming or layering requires it.

## Key Decision

MCP should integrate the externally reusable open API capabilities, not only payout.

That means MCP should be the stable target for:

- payment capabilities
- payout capabilities
- balance capabilities
- query capabilities
- callback and notification handling guidance
- common signing, auth, idempotency, and error conventions

At the same time, merchant onboarding, KYB/KYC, internal wallet management, settlement internals, and ledger internals remain preconditions or lower-layer systems, not first-phase MCP runtime domains.

## Architecture Positioning

The architecture should be understood as four layers:

### 1. AI and protocol entry layer

Examples:

- `skill`
- `x402`
- future payment protocol adapters

Responsibilities:

- accept a trigger or standardized request form
- guide or normalize the caller into the right capability path
- call MCP rather than implement business logic directly

### 2. MCP capability layer

This is the single maintained open runtime capability layer.

Responsibilities:

- environment selection model
- API credential usage model
- signing conventions
- request normalization
- capability routing
- consistent external contract behavior

### 3. Open business capability layer

Examples:

- create payment
- query payment
- cancel payment
- create payout
- query payout
- query balance
- callback verification or consumption guidance

These capabilities are exposed through MCP and documented through `TemarAISkill`.

### 4. Internal platform implementation layer

Examples:

- merchant registration
- KYB/KYC
- internal wallet management
- settlement and ledger internals
- fund routing implementation
- on-chain execution implementation

These remain lower-layer systems or preconditions and are not the main focus of first-phase public MCP runtime documentation.

## What MCP Should Cover in Phase One

MCP phase one should focus on open capabilities that external integrators or AI assistants actually need at runtime:

- `common`
  - environment selection
  - API key usage
  - secret key usage
  - signing inputs
  - request IDs
  - error conventions
  - idempotency expectations

- `payment`
  - create payment
  - query payment
  - cancel payment

- `payout`
  - create payout
  - query payout

- `balance`
  - query balance

- `notification`
  - callback handling guidance
  - notification consumption expectations

## What MCP Should Not Center in Phase One

These areas are important platform capabilities, but they should not define the first-phase public MCP runtime model:

- merchant registration
- KYB/KYC process steps
- API key issuance workflows
- wallet account opening
- settlement internals
- fee ledger internals
- freeze and unfreeze bookkeeping internals
- internal reconciliation procedures

These are either:

- preconditions before MCP runtime usage
- lower-layer implementation details
- internal operations concerns rather than open capability entrypoints

## Runtime Integration Model

The intended runtime experience should be simple:

1. The merchant has already completed prerequisite onboarding and access enablement.
2. The caller chooses an environment such as sandbox or production.
3. The caller configures `apiKey`, `secretKey`, timestamp, request ID, and signature inputs.
4. The caller invokes MCP capabilities such as payment, payout, balance, or query.

This means public documentation should not make runtime users walk through merchant registration or internal enablement steps as if they are part of normal per-request execution.

## Role of `skill`

`skill` should be positioned as:

- an AI-facing integration guidance layer
- a routing and context layer
- a trigger layer that helps callers invoke the right MCP capability

`skill` should not be positioned as:

- the primary business execution layer
- the location where payout or payment business logic is maintained
- a competing runtime API stack alongside MCP

## Role of `x402`

`x402` should be positioned as:

- a protocol adapter or trigger surface
- a standardized invocation mechanism when applicable
- a caller-facing entry pattern that routes into MCP

It should not duplicate the capability logic already modeled in MCP.

## Documentation Consequences

If this architecture is adopted, future `TemarAISkill` documentation should evolve toward MCP-centered public organization.

That means the repository should gradually include or reorganize around topics such as:

- MCP overview
- environment selection
- authentication and signing
- payment capabilities
- payout capabilities
- balance capabilities
- query capabilities
- notifications and callbacks
- common errors and status mappings

The current `docs/skill` content can remain useful, but it should increasingly be interpreted as the public guidance layer that explains how to use MCP-backed capabilities rather than as protocol-specific business silos.

## Alternatives Considered

### Option 1: Make `skill` the primary integration core

Pros:

- fast to explain at first
- feels close to AI assistant usage

Cons:

- mixes guidance with execution
- causes protocol-specific duplication
- weakens long-term architecture boundaries

### Option 2: Make MCP payout-only

Pros:

- narrow first implementation scope
- easier to ship one domain quickly

Cons:

- other domains repeat the same architecture problem in future expansions
- encourages fragmented future protocol stacks
- fails to create a true single capability base

### Option 3: Make MCP the unified open capability base

Pros:

- strongest long-term maintainability
- one capability model for many entry protocols
- best fit for AI-facing documentation reuse

Cons:

- requires clearer up-front domain boundary decisions
- may need gradual repository evolution rather than one immediate rewrite

Recommendation: Option 3.

## Acceptance Criteria

This architecture direction is successful when:

1. MCP is defined as the single open capability base for runtime integration.
2. `skill` is clearly positioned as a trigger and guidance layer rather than the business core.
3. `x402` and future protocols are clearly positioned as adapters above MCP.
4. Runtime users only need to think about environment selection, credentials, signing, and capability invocation.
5. Merchant onboarding and internal wallet or ledger concerns are clearly treated as prerequisites or lower-layer systems.

## Risks and Mitigations

### Risk: MCP scope grows into an internal platform mirror

Mitigation:

- define phase-one MCP around externally reusable runtime capabilities only
- keep internal onboarding, settlement, and wallet internals outside first-phase public MCP scope

### Risk: `skill` and MCP remain conceptually mixed

Mitigation:

- document `skill` as the AI-facing guidance and trigger layer
- document MCP as the only maintained open capability runtime layer

### Risk: public docs remain organized around old domain assumptions

Mitigation:

- evolve the repository gradually toward MCP-centered guidance
- keep current assets but reinterpret them through the new architecture model

## Next Step

After this design is approved, create a follow-up implementation plan for updating the repository structure and top-level documentation so the MCP-first architecture becomes the new official framing inside `TemarAISkill`.
