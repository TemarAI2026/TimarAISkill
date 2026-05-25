# MCP Implementation Repository Design

## Goal

Define the first implementation-oriented design for the future MCP repository that will expose Timar payment capabilities to AI coding assistants and MCP clients without re-implementing business logic.

The core principle is:

**MCP is a thin adapter layer above existing public services, not a second payment core.**

## Non-Goals

This MCP repository does not:

- re-implement payment or payout business logic
- own merchant onboarding, KYB, KYC, settlement, ledger, or wallet internals
- become a replacement for the existing public API platform
- define a second source of truth for status transitions or fee rules

If a business rule already exists in the current public service, MCP should call that service rather than duplicate the rule locally.

## Recommended stack

Use `Node.js + TypeScript` for the MCP implementation repository.

Reasons:

- it fits the current MCP and AI tool ecosystem well
- tool schemas and JSON-first transport are straightforward
- it is a good match for a thin adapter and router layer
- future protocol adapters such as `x402` can be added above the same runtime shape

This is a tooling-layer choice, not a signal that the payment core should move out of its current backend stack.

## Repository role

The future MCP repository should be understood as:

- an MCP server
- a tool surface for AI clients
- a configuration and environment adapter
- a signing and request-header adapter
- a capability router
- a client layer that calls existing public Timar APIs

It should not be understood as:

- a payment engine
- a ledger service
- a direct business-rule execution layer

## Thin Adapter Architecture

The repository should follow this flow:

1. MCP client calls a tool
2. tool input is validated
3. runtime environment and credentials are resolved
4. auth adapter generates timestamp, request ID, and signature
5. capability router maps the tool call to the right API client method
6. API client calls the existing public Timar service
7. MCP response adapter returns a normalized tool result

The only local logic should be:

- input validation
- environment resolution
- signing and header construction
- endpoint selection
- response normalization
- safe error and retry boundaries

## Server Shape

The server should be one MCP server process with modular capability registration.

Recommended shape:

- one server entrypoint
- one shared runtime context
- one tool registry
- one capability router layer
- one shared API client base

At startup, the server should:

1. load configuration
2. validate required environment definitions
3. register available capability tools
4. expose health or diagnostics metadata for debugging

## Tool Surface

The first-phase tool surface should map closely to the current public capability set.

Recommended initial tools:

- `payment.create`
- `payment.get`
- `payment.cancel`
- `payout.create`
- `payout.get`
- `balance.list`

Second-wave tools can include:

- `notification.verify`
- `notification.normalize`
- `diagnostics.ping`
- `diagnostics.resolveConfig`

### Tool design rule

Each tool should be:

- capability-specific
- explicit in required input fields
- narrow in purpose
- mapped to one public API path or one tightly related operation

Avoid one generic `request` tool. That would reintroduce the same ambiguity MCP is meant to remove.

## Tool Input Model

Each tool input should contain:

- `environment`
- capability-specific business fields
- client trace metadata only when the caller has a real need to correlate upstream activity

Do not expose signing fields such as `timestamp`, `requestId`, or `sign` as normal tool inputs. Those belong to the auth adapter, not the MCP caller.

Example direction:

- `payment.create` accepts business input like `merchantOrderId`, `merchantUserId`, `amount`, `currency`, `network`, `returnUrl`, `cancelUrl`
- `payout.create` accepts business input like `merchantOrderId`, `merchantUserId`, `amount`, `currency`, `network`, `withdrawAddress`
- `payment.get` and `payout.get` accept `orderId`

## Tool Output Model

Tool outputs should preserve enough raw information for troubleshooting while still being easy for AI to consume.

Recommended structure:

```json
{
  "ok": true,
  "environment": "sandbox",
  "requestId": "req-20260525-0001",
  "code": "0",
  "message": "",
  "data": {}
}
```

Output rules:

- preserve the original platform `code`
- preserve the original platform `msg`
- preserve the generated request trace ID
- return business payload in `data`
- use a top-level `ok` convenience flag for MCP clients

## Config Model

Configuration should be environment-first.

Recommended top-level config model:

```text
environments:
  sandbox:
    baseUrl
    apiKey
    secretKey
    timeoutMs
  production:
    baseUrl
    apiKey
    secretKey
    timeoutMs
defaultEnvironment
```

### Config principles

- every environment owns its own credentials
- do not share one credential bundle across environments
- allow environment disabling when a capability is not ready
- keep secrets out of logs

Future config sources can include:

- local file
- environment variables
- secret manager

But phase one should standardize one canonical in-process config shape first.

## Auth Adapter

The auth adapter is one of the most important local layers because it removes signing ambiguity from AI callers.

Responsibilities:

- generate `X-Api-Timestamp`
- generate fresh `X-Api-RequestId`
- build signing payload
- produce `X-Api-Sign`
- attach required headers

It should implement the current public rule:

- with body: `timestamp + requestId + rawBody`
- without body: `timestamp + requestId`
- sign with `Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`

### Auth adapter design rule

The adapter should always sign the exact outbound raw body string. It should not let downstream code mutate the body after signing.

## Capability Router

The capability router maps a tool call to the correct client operation.

Recommended routing shape:

- `payment` router
- `payout` router
- `balance` router
- `notification` router in a later phase when the public contract is stable enough

Each router should:

- validate the expected input shape
- choose the API method and path
- delegate request execution to the shared client layer
- return normalized output

The router should not decide merchant business policy, approval policy, or ledger behavior.

## API Client Layer

The API client layer is the only place that should actually perform outbound HTTP requests.

Recommended layering:

- `BaseApiClient`
  handles JSON serialization, headers, timeouts, HTTP execution, wrapper parsing
- `PaymentApiClient`
  owns create/get/cancel payment calls
- `PayoutApiClient`
  owns create/get payout calls
- `BalanceApiClient`
  owns balance listing calls

This preserves one direction:

`tool -> router -> api client -> public service`

## Error Model

The MCP repository should not hide platform failures.

Recommended behavior:

- preserve public `code` and `msg`
- add local `ok` as a convenience
- attach `requestId`
- separate transport failure from business failure

Suggested local categories:

- `transport_error`
- `auth_error`
- `business_error`
- `config_error`
- `validation_error`

But the original public response should remain visible to callers.

## Retry Model

The MCP repository should be conservative about retries.

Rules:

- do not blindly retry create-payment
- do not blindly retry create-payout
- allow safe re-query tools to be called repeatedly
- surface duplicate-order scenarios clearly
- prefer investigation with saved `requestId` and business identifiers before retrying creates

Automatic retries should be limited to transport-safe cases and should not become default behavior for create operations.

## Repository Structure

Recommended implementation repository structure:

```text
src/
  server/
    index.ts
    register-tools.ts
  config/
    config-schema.ts
    config-loader.ts
    environment-resolver.ts
  auth/
    signing.ts
    header-builder.ts
    request-id.ts
  clients/
    base-api-client.ts
    payment-api-client.ts
    payout-api-client.ts
    balance-api-client.ts
  routers/
    payment-router.ts
    payout-router.ts
    balance-router.ts
  tools/
    payment-create.ts
    payment-get.ts
    payment-cancel.ts
    payout-create.ts
    payout-get.ts
    balance-list.ts
  models/
    tool-inputs.ts
    tool-outputs.ts
    public-api-types.ts
  errors/
    error-types.ts
    normalize-error.ts
  utils/
    json.ts
    logging.ts
    redact.ts
docs/
tests/
```

## Extension Model

This structure should make future growth possible without changing the core rule that MCP stays thin.

Later extensions can include:

- `notification` tool set
- `fiat` capability routers
- `x402` adapter layer above MCP
- alternate credential providers
- additional diagnostics tools

The extension rule is:

**new capabilities add new routers and clients; they do not change MCP into a second business platform.**

## Security and Logging Rules

- never log `secretKey`
- redact signature values unless explicitly needed by security policy
- log `requestId`, environment, method, path, response `code`, and response `msg`
- keep raw body logging configurable

## Testing Strategy

The implementation repository should be tested in layers:

1. unit tests for signing and header generation
2. unit tests for config resolution
3. unit tests for tool input validation
4. mocked router-to-client tests
5. integration tests against sandbox configuration later

The first tests should focus on:

- exact signing payload behavior
- correct endpoint routing
- config isolation by environment
- response normalization

## Phase-One Implementation Sequence

Recommended build order:

1. config schema and loader
2. auth adapter
3. base API client
4. payment router and tools
5. payout router and tools
6. balance router and tool
7. error normalization and diagnostics

This sequence keeps the thin-adapter rule intact and gets the highest-value capabilities online first.

## Open Design Decision

The main unresolved implementation choice after this design is:

- whether the MCP implementation repository should live in this repository later, or move to a separate runtime repository once code starts

The design above works in either case because it describes runtime shape, not repository ownership policy.

## Bottom Line

The future MCP implementation repository should be:

- thin
- environment-first
- capability-specific
- signing-safe
- router-driven
- built on top of existing public Timar APIs

It should not become a second payment core.
