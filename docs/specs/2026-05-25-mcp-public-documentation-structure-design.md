# MCP Public Documentation Structure Design

## Summary

This design defines the first public documentation structure for MCP-centered open integration guidance inside `TemarAISkill`.

The repository already has:

- imported public capability documentation under `docs/skill`
- repository-governance and maintenance documents
- an MCP-first architecture framing

The next step is to introduce a clear MCP-oriented public documentation skeleton so future content can grow around MCP directly rather than only around the earlier crypto-first document framing.

## Goals

- Add a public MCP documentation skeleton inside this repository.
- Keep the current `docs/skill` content usable while introducing a clearer MCP-centered entry model.
- Separate MCP-wide common concepts from capability-specific guides.
- Support external integrators and AI coding assistants with a stable top-down MCP reading path.
- Keep first-phase scope focused on the currently published capability set.

## Non-Goals

- This phase does not rewrite every existing crypto document.
- This phase does not remove the current `docs/skill` structure.
- This phase does not publish internal onboarding, ledger, or wallet implementation docs.
- This phase does not require protocol-specific docs for `x402` or other future adapters yet.

## Design Principle

Introduce MCP documentation as a new public framing layer, while reusing current `docs/skill` assets as the first published capability backing.

This means:

- MCP gets its own explicit overview and capability navigation
- existing crypto capability docs remain the first implemented capability domain
- future adapters such as `skill` and `x402` can reference MCP docs instead of duplicating business guidance

## Recommended Structure

Add a new MCP-focused public area under `docs/mcp/`.

Recommended first-phase structure:

```text
docs/mcp/
  README.md
  README.en.md
  README.zh-TW.md

  overview/
    architecture.md
    architecture.en.md
    architecture.zh-TW.md
    environments.md
    environments.en.md
    environments.zh-TW.md
    auth-signing.md
    auth-signing.en.md
    auth-signing.zh-TW.md

  capabilities/
    payment.md
    payment.en.md
    payment.zh-TW.md
    payout.md
    payout.en.md
    payout.zh-TW.md
    balance.md
    balance.en.md
    balance.zh-TW.md
    notifications.md
    notifications.en.md
    notifications.zh-TW.md

  references/
    common.md
    common.en.md
    common.zh-TW.md
```

## Why a new `docs/mcp/` area

### Benefits

- gives MCP a clear public home
- avoids overloading the existing `docs/skill` structure
- lets current `docs/skill` docs continue serving as AI-facing capability guidance
- creates a clean path for future protocol docs to point into MCP

### Trade-off

- there will be a temporary period where both `docs/skill` and `docs/mcp` exist

This is acceptable because the two areas serve different roles during transition:

- `docs/mcp/`: MCP-first public capability model
- `docs/skill/`: AI-facing guidance and capability task docs built on top of MCP framing

## First-Phase MCP Document Responsibilities

### `docs/mcp/README*`

Purpose:

- explain what MCP is
- explain who it is for
- explain what prerequisites are assumed
- route readers into overview and capability pages

### `overview/architecture*`

Purpose:

- explain MCP as the open capability base
- explain relationship to `skill`, `x402`, and future adapters
- explain what MCP does and does not own

### `overview/environments*`

Purpose:

- explain environment selection
- explain runtime expectations for sandbox and production
- explain that merchant onboarding is a prerequisite rather than a runtime step

### `overview/auth-signing*`

Purpose:

- explain runtime credentials
- explain `apiKey`, `secretKey`, timestamp, request ID, and signature model
- point to current shared references where exact facts already exist

### `capabilities/payment*`

Purpose:

- explain the payment capability set currently backed by the crypto published docs
- point to current published create/query/cancel material

### `capabilities/payout*`

Purpose:

- explain the payout capability set currently backed by the crypto published docs
- point to current published create/query material

### `capabilities/balance*`

Purpose:

- explain the current balance capability
- point to current published balance docs

### `capabilities/notifications*`

Purpose:

- explain callback and asynchronous notification handling expectations
- point to current webhook guidance

### `references/common*`

Purpose:

- summarize common capability references at a high level
- point readers to exact source-backed references in `docs/skill/references`

## Relationship to Existing `docs/skill`

The existing `docs/skill` content should remain in place.

Interpretation after this change:

- `docs/mcp/` becomes the explicit MCP-first public entrypoint
- `docs/skill/` remains the AI-facing implementation guidance layer
- `docs/skill/references` remains the exact fact source for currently published public contract details until a future consolidation step is deliberately planned

## Language Strategy

The first-phase MCP docs should follow the same language convention used elsewhere in the repository:

- default `.md`: Simplified Chinese
- `.en.md`: English
- `.zh-TW.md`: Traditional Chinese

Rules:

- keep structure identical across languages
- keep identifiers and code-formatted values identical across languages
- vary only explanatory prose

## Scope Boundary

First-phase MCP public docs should cover:

- MCP positioning
- environment selection
- auth and signing
- payment capability navigation
- payout capability navigation
- balance capability navigation
- notification capability navigation

They should not yet cover:

- merchant onboarding workflows as runtime docs
- settlement or ledger internals
- wallet internals
- adapter-specific runtime documents for `x402`

## Acceptance Criteria

This design is successful when:

1. The repository gains a clear MCP public documentation home.
2. External readers can start from MCP, then route into the published capability docs.
3. The new MCP docs do not duplicate exact contract facts unnecessarily.
4. The current `docs/skill` tree remains usable during transition.
5. The new structure leaves room for future adapter-facing documents without mixing them into the MCP core.

## Risks and Mitigations

### Risk: duplication between `docs/mcp` and `docs/skill`

Mitigation:

- keep `docs/mcp` high-level and routing-oriented in phase one
- continue using `docs/skill/references` for exact contract facts

### Risk: readers do not understand which entrypoint to use

Mitigation:

- update repository root docs to point clearly at MCP as the primary capability base
- explain the role split between `docs/mcp` and `docs/skill`

### Risk: scope expands too early

Mitigation:

- keep first phase limited to overview plus capability routing
- defer full domain rewrites or protocol-specific docs

## Next Step

After this design is approved, create an implementation plan for adding the first-phase `docs/mcp` skeleton and updating repository entrypoints to expose it.
