# MCP Architecture

## Purpose

This page explains how MCP should be understood by external integrators and AI coding assistants. The goal is not to describe every internal system, but to define the public capability boundary clearly enough that callers do not invent extra layers or unsupported runtime behavior.

## MCP role

MCP is the unified open capability base for the Temar platform.

Its public responsibilities are:

- providing one consistent open capability entrypoint
- unifying the runtime calling model
- unifying signing, request identity, wrapper semantics, and capability routing
- giving higher-level tools and protocol adapters one stable execution base

## Relationship to upper layers

In the current architecture:

- `skill` is the AI-facing guidance and trigger layer
- `x402` is a protocol adapter layer
- future protocol surfaces should also be adapter layers above MCP

These upper entry surfaces should not maintain separate business cores when they need the same payment or payout behavior. They should route into MCP and reuse the same runtime contract.

## Phase-one ownership

In phase one, MCP owns the public capability surface for:

- payment
- payout
- balance
- notifications
- shared runtime calling conventions

This means an external integration should model MCP as the runtime API layer for money-movement-related open capabilities that are already published.

## What MCP does not own in phase one

MCP is not the public owner of:

- merchant registration workflows
- the KYB or KYC process itself
- internal wallet implementation details
- settlement or ledger internals
- internal reconciliation procedures
- internal operations approval processes

These remain either prerequisites to runtime usage or lower-layer platform concerns outside the current public MCP contract.

## AI assistant modeling rule

When an AI coding assistant reads this documentation, it should reason about MCP as:

- the place to call published capabilities
- the place to apply shared signing and response rules
- not the place to guess internal merchant, ledger, or wallet behavior

If a required fact is not present in MCP docs or the linked exact references, the assistant should not invent it.

## Recommended mental model

Use this layering model when planning integrations:

1. Business onboarding happens before runtime usage.
2. MCP handles runtime capability calls.
3. `skill` improves how AI assistants understand and invoke MCP.
4. `x402` or future protocol layers adapt their own surface into MCP.

This keeps public integration logic centralized and reduces drift between human-written clients, AI-generated clients, and future protocol adapters.

## Common mistakes

- treating MCP as a full merchant lifecycle platform instead of a runtime capability layer
- pushing internal ledger or wallet assumptions into public client code
- letting every upper-layer adapter define its own signing and status semantics
- duplicating payment or payout business logic outside MCP when the goal is only to change the access surface

## Related pages

- [`./environments.en.md`](./environments.en.md)
- [`./auth-signing.en.md`](./auth-signing.en.md)
- [`../capabilities/payment.en.md`](../capabilities/payment.en.md)
- [`../capabilities/payout.en.md`](../capabilities/payout.en.md)
