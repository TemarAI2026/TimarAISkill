# Environments

## Runtime model

At runtime, MCP integration focuses on only a small set of actions:

- choose the environment
- configure credentials
- generate signing inputs
- call capabilities

## Supported environment idea

The current docs assume at least these environment concepts:

- `sandbox`
- `production`

Actual base URLs, enabled capabilities, and release rules should follow the formal platform configuration and the currently published references.

## Prerequisites

Environment switching is not part of onboarding.  
Before runtime usage, the following should already be complete:

- merchant registration
- access enablement
- API credential issuance

## Operational guidance

- manage `apiKey` and `secretKey` separately per environment
- do not mix sandbox and production request parameters
- re-verify signing, status flows, and callback behavior in the target environment before release
