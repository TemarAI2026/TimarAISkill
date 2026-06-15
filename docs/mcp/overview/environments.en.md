# Environments

## Purpose

This page defines the phase-one runtime environment model for MCP integrations. Its purpose is to keep external callers and AI coding assistants from mixing credentials, assumptions, and release behavior across environments.

## Runtime model

At runtime, MCP integration focuses on a small set of actions:

- choose the environment
- load the environment-specific credentials
- generate the signing inputs
- call the published capabilities
- verify the returned status and identifiers in the same environment

Environment switching is a runtime configuration concern, not a merchant-onboarding concern.

## Current environment concepts

The current public docs assume at least these environment concepts:

- `sandbox`
- `production`

Actual base URLs, enabled capabilities, release windows, and operational rules should follow formal platform configuration and the current exact published references.

## Minimum environment-specific configuration

Each environment should be modeled with its own configuration set, including at least:

- base URL
- `apiKey`
- `secretKey`
- request timeout policy
- logging or trace policy

Do not share one credential bundle across sandbox and production.

## Prerequisites

Before runtime environment usage begins, the following should already be complete:

- merchant registration
- access enablement
- API credential issuance

MCP runtime docs do not replace these onboarding steps.

## Isolation rules

Keep these isolation rules in place:

- manage `apiKey` and `secretKey` separately per environment
- do not mix sandbox and production request parameters
- do not reuse stored `orderId` values across environments
- do not assume a capability is enabled in production because it exists in sandbox
- do not treat sandbox status behavior as sufficient production evidence without re-verification

## Release checklist

Before releasing an MCP integration to production, re-verify in the target environment:

1. signing behavior
2. required headers
3. endpoint paths and methods
4. status handling and follow-up logic
5. notification or callback behavior, if used
6. logging and trace retention

## Common mistakes

- using the wrong `apiKey` and `secretKey` pair for the selected environment
- calling production with sandbox base URLs or vice versa
- promoting client code without rechecking environment-specific status or callback behavior
- storing environment-agnostic order lookups that can accidentally cross environments

## Related pages

- [`./architecture.en.md`](./architecture.en.md)
- [`./auth-signing.en.md`](./auth-signing.en.md)
- [`../references/common.en.md`](../references/common.en.md)
