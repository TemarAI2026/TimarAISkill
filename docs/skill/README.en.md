# AI Integration Docs

## Positioning

This directory is the public entrypoint for AI coding assistants and external integrators. It helps you understand the VGPAY integration doc set, available domains, and the recommended reading order before implementation.

## Who this is for

- Developers using Claude Code, Cursor, or Codex to assist with payment integrations
- External integrators who need to find domain docs, references, and implementation constraints quickly
- Engineering teams that want the right context in place before generating code

## Supported AI coding assistants

- Claude Code
- Cursor
- Codex

## Quick start

Before generating or changing integration code, read the docs in this order:

1. Start with [`hub/integration-router.en.md`](./hub/integration-router.en.md) to identify the right entrypoint and reading path.
2. Continue with [`hub/domain-map.en.md`](./hub/domain-map.en.md) to understand the active domains and doc layout.
3. Move into the domain docs. The currently implemented domain is [`domains/crypto/overview.en.md`](./domains/crypto/overview.en.md).
4. Review the shared rules, including [`domains/shared/auth-signing.en.md`](./domains/shared/auth-signing.en.md), [`domains/shared/error-handling.en.md`](./domains/shared/error-handling.en.md), and [`domains/shared/response-conventions.en.md`](./domains/shared/response-conventions.en.md).
5. Read the reference material before code generation, such as [`references/crypto/integration-checklist.en.md`](./references/crypto/integration-checklist.en.md), [`references/crypto/endpoints.en.md`](./references/crypto/endpoints.en.md), and [`references/shared/headers.en.md`](./references/shared/headers.en.md).

## Current domain availability

- `Crypto`: implemented today and ready for integration planning and code generation.
- `Fiat`: reserved as a future domain entrypoint and should not be treated as implemented yet.

## Directory guide

- [`hub/`](./hub/): overview entrypoints, routing guidance, and domain mapping.
- [`domains/`](./domains/): implementation guidance organized by domain.
- [`domains/crypto/`](./domains/crypto/): the currently available crypto integration docs.
- [`domains/fiat/`](./domains/fiat/): reserved location for future fiat docs.
- [`domains/shared/`](./domains/shared/): cross-domain rules for authentication, error handling, and response conventions.
- [`references/`](./references/): endpoints, data models, statuses, headers, and integration checklists.
- [`examples/`](./examples/): example assets that can support implementation understanding.

## Important usage note

These docs are intended to establish the right context before code generation. Read the router and domain guidance first, then combine the shared rules and references when implementing; do not generate integration logic from a single endpoint page alone.
