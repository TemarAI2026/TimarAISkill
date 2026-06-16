# MCP Implementation Repository Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the first thin-adapter MCP runtime repository shape for Temar that exposes payment, payout, and balance capabilities by calling existing public APIs rather than re-implementing business logic.

**Architecture:** The runtime is a thin `Node.js + TypeScript` MCP server with explicit tools, environment-first config, one auth adapter, one capability router layer, and API clients that call existing Temar public services. All business execution stays in the existing public services.

**Tech Stack:** Node.js, TypeScript, MCP server SDK/runtime, JSON schema validation, HTTP client library, test runner

---

## File Structure

The implementation repository should create these units first:

- `src/server/index.ts`
  server bootstrap and runtime startup
- `src/server/register-tools.ts`
  central tool registration
- `src/config/config-schema.ts`
  config type and validation schema
- `src/config/config-loader.ts`
  config loading and normalization
- `src/config/environment-resolver.ts`
  environment lookup and per-request environment resolution
- `src/auth/request-id.ts`
  request ID generation
- `src/auth/signing.ts`
  raw payload signing logic
- `src/auth/header-builder.ts`
  header construction for outbound requests
- `src/clients/base-api-client.ts`
  outbound HTTP execution, wrapper parsing, shared request pipeline
- `src/clients/payment-api-client.ts`
  payment API methods
- `src/clients/payout-api-client.ts`
  payout API methods
- `src/clients/balance-api-client.ts`
  balance API methods
- `src/routers/payment-router.ts`
  payment tool routing
- `src/routers/payout-router.ts`
  payout tool routing
- `src/routers/balance-router.ts`
  balance tool routing
- `src/tools/payment-create.ts`
  MCP tool definition
- `src/tools/payment-get.ts`
  MCP tool definition
- `src/tools/payment-cancel.ts`
  MCP tool definition
- `src/tools/payout-create.ts`
  MCP tool definition
- `src/tools/payout-get.ts`
  MCP tool definition
- `src/tools/balance-list.ts`
  MCP tool definition
- `src/models/tool-inputs.ts`
  tool input types and validation models
- `src/models/tool-outputs.ts`
  normalized MCP output types
- `src/models/public-api-types.ts`
  shared public wrapper and endpoint payload types
- `src/errors/error-types.ts`
  local error categories
- `src/errors/normalize-error.ts`
  translate transport/config/auth failures into MCP-safe output
- `src/utils/json.ts`
  exact JSON serialization helpers
- `src/utils/logging.ts`
  shared logger interface
- `src/utils/redact.ts`
  secret-safe logging helpers
- `tests/unit/auth/*.test.ts`
  signing and header tests
- `tests/unit/config/*.test.ts`
  config loading and resolution tests
- `tests/unit/routers/*.test.ts`
  mocked router tests
- `tests/unit/tools/*.test.ts`
  input validation and shape tests

## Task 1: Bootstrap Runtime Skeleton

**Files:**
- Create: `src/server/index.ts`
- Create: `src/server/register-tools.ts`
- Create: `src/models/tool-outputs.ts`
- Create: `src/errors/error-types.ts`

- [ ] **Step 1: Create the runtime skeleton files**

Define the minimal module boundaries:

- `index.ts` owns startup
- `register-tools.ts` owns tool registration only
- `tool-outputs.ts` owns the normalized MCP result shape
- `error-types.ts` owns local error categories

- [ ] **Step 2: Define the normalized MCP output contract**

Add a shared output shape with:

- `ok`
- `environment`
- `requestId`
- `code`
- `message`
- `data`

Acceptance:

- the type is reusable by routers and tools
- it preserves upstream `code` and `msg`

- [ ] **Step 3: Define local error category types**

Create local error categories for:

- `transport_error`
- `auth_error`
- `business_error`
- `config_error`
- `validation_error`

- [ ] **Step 4: Wire the initial empty server bootstrap**

The server should start with an empty or initial registration path, but it should already separate:

- config loading
- tool registration
- startup execution

- [ ] **Step 5: Verify bootstrap compiles**

Run the repository typecheck or build command once the runtime package exists.

Expected:

- no import-cycle or missing-symbol failures in the bootstrap layer

## Task 2: Build Config Layer

**Files:**
- Create: `src/config/config-schema.ts`
- Create: `src/config/config-loader.ts`
- Create: `src/config/environment-resolver.ts`
- Test: `tests/unit/config/config-loader.test.ts`
- Test: `tests/unit/config/environment-resolver.test.ts`

- [ ] **Step 1: Define the config shape**

The config model must support:

- `environments.sandbox`
- `environments.production`
- `baseUrl`
- `apiKey`
- `secretKey`
- `timeoutMs`
- `defaultEnvironment`

- [ ] **Step 2: Write failing config validation tests**

Test cases:

- missing `defaultEnvironment` fails
- missing `baseUrl` fails
- missing `apiKey` fails
- missing `secretKey` fails
- unknown selected environment fails

- [ ] **Step 3: Implement config schema validation**

Implement the schema so invalid config fails fast at startup.

- [ ] **Step 4: Implement config loader**

The loader should:

- read one canonical config source shape
- validate it
- return normalized runtime config

- [ ] **Step 5: Implement environment resolver**

The resolver should:

- resolve explicit per-call `environment`
- fall back to `defaultEnvironment` when allowed
- return one exact environment config bundle

- [ ] **Step 6: Run config tests**

Expected:

- invalid configuration fails deterministically
- valid configuration resolves correctly per environment

## Task 3: Build Auth Adapter

**Files:**
- Create: `src/auth/request-id.ts`
- Create: `src/auth/signing.ts`
- Create: `src/auth/header-builder.ts`
- Create: `src/utils/json.ts`
- Test: `tests/unit/auth/signing.test.ts`
- Test: `tests/unit/auth/header-builder.test.ts`

- [ ] **Step 1: Write failing signing tests**

Test cases:

- with body: payload is `timestamp + requestId + rawBody`
- without body: payload is `timestamp + requestId`
- signature output is Base64 HMAC-SHA256
- changing JSON field order changes the raw body string and therefore the signature

- [ ] **Step 2: Implement exact raw-body serialization helper**

The helper must support:

- exact outbound JSON string generation before signing
- no mutation after signing

- [ ] **Step 3: Implement request ID generator**

Requirements:

- fresh per request
- log-safe
- traceable

- [ ] **Step 4: Implement signing function**

Requirements:

- accepts `secretKey`
- accepts `timestamp`
- accepts `requestId`
- accepts a nullable raw body string
- returns final signature string

- [ ] **Step 5: Implement header builder**

The builder should emit:

- `X-Api-Key`
- `X-Api-Timestamp`
- `X-Api-RequestId`
- `X-Api-Sign`

- [ ] **Step 6: Run auth tests**

Expected:

- all signing payload rules match the current published contract

## Task 4: Build Shared API Client Base

**Files:**
- Create: `src/clients/base-api-client.ts`
- Create: `src/models/public-api-types.ts`
- Create: `src/errors/normalize-error.ts`
- Create: `src/utils/logging.ts`
- Create: `src/utils/redact.ts`
- Test: `tests/unit/clients/base-api-client.test.ts`

- [ ] **Step 1: Define shared public response wrapper types**

Support:

- `code`
- `msg`
- `data`

- [ ] **Step 2: Write failing shared client tests**

Test cases:

- request headers are attached
- wrapper response is parsed correctly
- network failures become `transport_error`
- invalid config becomes `config_error`

- [ ] **Step 3: Implement base client request pipeline**

Responsibilities:

- build raw body string
- ask auth adapter for headers
- execute HTTP request
- parse wrapper response
- preserve upstream `code` and `msg`

- [ ] **Step 4: Implement safe logging and redaction helpers**

Requirements:

- never log `secretKey`
- allow request trace logging
- redact sensitive signature content when needed

- [ ] **Step 5: Implement local error normalization**

Convert transport/config/auth failures into a consistent local error object without hiding upstream business responses.

- [ ] **Step 6: Run shared client tests**

Expected:

- outbound request pipeline works with mocked HTTP responses

## Task 5: Build Capability-Specific API Clients

**Files:**
- Create: `src/clients/payment-api-client.ts`
- Create: `src/clients/payout-api-client.ts`
- Create: `src/clients/balance-api-client.ts`
- Test: `tests/unit/clients/payment-api-client.test.ts`
- Test: `tests/unit/clients/payout-api-client.test.ts`
- Test: `tests/unit/clients/balance-api-client.test.ts`

- [ ] **Step 1: Write failing payment client tests**

Test cases:

- create uses `POST /api/v2/digital/payments`
- get uses `GET /api/v2/digital/payments/{orderId}`
- cancel uses `POST /api/v2/digital/payments/{orderId}/cancel`

- [ ] **Step 2: Write failing payout client tests**

Test cases:

- create uses `POST /api/v2/digital/payouts`
- get uses `GET /api/v2/digital/payouts/{orderId}`

- [ ] **Step 3: Write failing balance client tests**

Test cases:

- list uses `GET /api/v2/digital/balances`

- [ ] **Step 4: Implement payment API client**

Methods:

- `createPayment`
- `getPayment`
- `cancelPayment`

- [ ] **Step 5: Implement payout API client**

Methods:

- `createPayout`
- `getPayout`

- [ ] **Step 6: Implement balance API client**

Methods:

- `listBalances`

- [ ] **Step 7: Run capability client tests**

Expected:

- all routes and methods match the current published endpoint contract exactly

## Task 6: Build Tool Input Models

**Files:**
- Create: `src/models/tool-inputs.ts`
- Test: `tests/unit/tools/tool-inputs.test.ts`

- [ ] **Step 1: Define payment tool input models**

Models:

- `PaymentCreateInput`
- `PaymentGetInput`
- `PaymentCancelInput`

- [ ] **Step 2: Define payout tool input models**

Models:

- `PayoutCreateInput`
- `PayoutGetInput`

- [ ] **Step 3: Define balance tool input model**

Model:

- `BalanceListInput`

- [ ] **Step 4: Add input validation tests**

Test cases:

- required payment fields enforced
- `withdrawAddress` required for payout create
- `environment` accepted and validated
- unknown extra control fields like `sign` or `timestamp` are rejected or ignored by policy

- [ ] **Step 5: Run input model tests**

Expected:

- tool inputs enforce the thin-adapter contract and do not expose signing inputs

## Task 7: Build Capability Routers

**Files:**
- Create: `src/routers/payment-router.ts`
- Create: `src/routers/payout-router.ts`
- Create: `src/routers/balance-router.ts`
- Test: `tests/unit/routers/payment-router.test.ts`
- Test: `tests/unit/routers/payout-router.test.ts`
- Test: `tests/unit/routers/balance-router.test.ts`

- [ ] **Step 1: Write failing payment router tests**

Test cases:

- validated input is routed to payment client create
- validated input is routed to payment client get
- validated input is routed to payment client cancel
- upstream wrapper response becomes normalized MCP output

- [ ] **Step 2: Write failing payout router tests**

Test cases:

- validated input is routed to payout client create
- validated input is routed to payout client get

- [ ] **Step 3: Write failing balance router tests**

Test cases:

- validated input is routed to balance client list

- [ ] **Step 4: Implement payment router**

Responsibilities:

- validate input
- resolve environment
- call payment client
- normalize output

- [ ] **Step 5: Implement payout router**

Responsibilities:

- validate input
- resolve environment
- call payout client
- normalize output

- [ ] **Step 6: Implement balance router**

Responsibilities:

- validate input
- resolve environment
- call balance client
- normalize output

- [ ] **Step 7: Run router tests**

Expected:

- routers perform no payment business logic and only orchestrate the thin runtime flow

## Task 8: Build MCP Tool Definitions

**Files:**
- Create: `src/tools/payment-create.ts`
- Create: `src/tools/payment-get.ts`
- Create: `src/tools/payment-cancel.ts`
- Create: `src/tools/payout-create.ts`
- Create: `src/tools/payout-get.ts`
- Create: `src/tools/balance-list.ts`
- Modify: `src/server/register-tools.ts`
- Test: `tests/unit/tools/payment-tools.test.ts`
- Test: `tests/unit/tools/payout-tools.test.ts`
- Test: `tests/unit/tools/balance-tools.test.ts`

- [ ] **Step 1: Write failing tool registration tests**

Test cases:

- `payment.create` is registered
- `payment.get` is registered
- `payment.cancel` is registered
- `payout.create` is registered
- `payout.get` is registered
- `balance.list` is registered

- [ ] **Step 2: Implement payment tool definitions**

Each tool should:

- expose a clear name
- bind to one router method
- define one narrow input contract

- [ ] **Step 3: Implement payout tool definitions**

Each tool should:

- expose a clear name
- bind to one router method
- define one narrow input contract

- [ ] **Step 4: Implement balance tool definition**

The tool should:

- expose a clear name
- bind to the balance router
- define one narrow input contract

- [ ] **Step 5: Wire central registration**

Register all first-phase tools in one place.

- [ ] **Step 6: Run tool registration tests**

Expected:

- the MCP server exposes the agreed thin-adapter tool surface and nothing generic like a raw request tunnel

## Task 9: Diagnostics and Final Runtime Wiring

**Files:**
- Modify: `src/server/index.ts`
- Modify: `src/server/register-tools.ts`
- Create: `tests/unit/server/index.test.ts`

- [ ] **Step 1: Add startup validation behavior**

The server should fail fast when:

- config is missing
- required environment definitions are invalid
- tool registration cannot complete

- [ ] **Step 2: Add minimal diagnostics metadata**

Expose at least:

- server version
- registered tool names
- configured environment names without secrets

- [ ] **Step 3: Add server bootstrap tests**

Test cases:

- valid config starts
- invalid config fails
- expected tools are registered

- [ ] **Step 4: Run server tests**

Expected:

- the bootstrap layer correctly assembles the runtime

## Task 10: Verification and Delivery

**Files:**
- Verify: repository-wide source and test files
- Modify: docs if implementation drift requires it

- [ ] **Step 1: Run unit test suite**

Run the repository unit tests.

Expected:

- all auth, config, client, router, tool, and server tests pass

- [ ] **Step 2: Run typecheck or build**

Run the repository typecheck/build command.

Expected:

- zero type errors

- [ ] **Step 3: Re-check the implementation against the design spec**

Confirm:

- no business logic was moved into MCP
- all first-phase tools map to existing public APIs
- config remains environment-first
- signing remains internal to the auth adapter

- [ ] **Step 4: Commit in focused slices**

Recommended commit boundaries:

- bootstrap + config
- auth + shared client
- capability clients
- routers + tools
- diagnostics + tests

## Self-Review

Spec coverage:

- server shape is covered by Tasks 1, 8, and 9
- tool surface is covered by Tasks 6, 7, and 8
- config model is covered by Task 2
- auth adapter is covered by Task 3
- capability router is covered by Task 7
- API client layering is covered by Tasks 4 and 5
- error and retry handling is covered by Tasks 4 and 10
- testing strategy is covered by all tasks with explicit verification steps

Placeholder scan:

- no unfinished-content markers
- no unresolved deferred-implementation markers

Type consistency:

- tool names match the design spec
- environment-first config shape is consistent through config, routers, and tools
- output contract stays consistent around `ok`, `environment`, `requestId`, `code`, `message`, and `data`
