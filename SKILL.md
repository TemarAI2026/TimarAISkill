# Timar AI Payment & Payout Skill

> **Skill Layer** — triggers and guides AI assistants to invoke Timar capabilities through MCP.

## Architecture

```
┌─────────────────────────────────────┐
│         Skill (this file)           │  ← AI-facing trigger & guide layer
│    Understand intent → Route        │
└──────────────┬──────────────────────┘
               │ invoke via MCP protocol
               ▼
┌─────────────────────────────────────┐
│      TimarAIMCP (MCP Server)        │  ← Capability execution layer
│   payment / payout / balance tools  │
└──────────────┬──────────────────────┘
               │ HTTP + HMAC-SHA256 sign
               ▼
┌─────────────────────────────────────┐
│       Timar Public API              │  ← Business backend
│   /api/v2/digital/*                 │
└─────────────────────────────────────┘
```

**You are in the Skill layer.** Your job is to understand user intent, select the right MCP tool, format correct parameters, and interpret results for the user. You do NOT call Timar APIs directly — you call MCP tools, and the MCP server handles signing, routing, and API communication.

## Prerequisites

Before using any MCP tool, confirm:

1. The MCP server (`TimarAIMCP`) is running and connected to this session.
2. Credentials are configured (apiKey, secretKey) in the MCP server config.
3. The target environment (sandbox/production) is set.

If MCP tools are not available, instruct the user to:
```bash
# Clone and configure TimarAIMCP
git clone <repo-url> TimarAIMCP && cd TimarAIMCP && npm install

# Run interactive setup wizard (supports zh-CN / zh-TW / en)
npm run setup

# Add to your AI client config pointing to scripts/start-stdio-server.mjs
```

## Available MCP Tools

### 1. `payment.create` — Create a payment (collection) order

**Use when:** The user wants to receive crypto payment from a customer.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |
| `merchantOrderId` | string | Your unique order ID |
| `merchantUserId` | string | Your user identifier |
| `amount` | number | Payment amount |
| `currency` | string | Currency code (e.g., `USDT`) |
| `network` | string | Blockchain network (e.g., `ethereum`, `tron`) |

**Optional parameters:**
| Parameter | Description |
|-----------|-------------|
| `returnUrl` | URL to redirect after payment |
| `cancelUrl` | URL to redirect on cancel |

**Key response fields to surface to user:**
- `orderId` — Platform order ID (save this!)
- `paymentUrl` — Payment page URL for customer
- `receiveAddress` — Deposit address
- `expiresInSeconds` — How long until expiry

**Common mistakes to avoid:**
- Do not invent `callbackUrl` — it is not a public field
- Do not confuse `merchantOrderId` with platform `orderId`
- Do not mix `currency` and `network` into one field
- Do not display expired payment links (check `expiresInSeconds`)

---

### 2. `payment.get` — Query a payment order status

**Use when:** User wants to check status of an existing payment order.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |
| `orderId` | string | Platform `orderId` (NOT merchantOrderId) |

**Key response fields:**
- `status`: `PENDING` | `SUCCESS` | `CANCEL` | `RISK`
- `paidAmount` — Actual paid amount
- `fee` / `feeCurrency`
- `depositDetails` — On-chain deposit info

---

### 3. `payment.cancel` — Cancel a payment order

**Use when:** User wants to cancel an unpaid payment order.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |
| `orderId` | string | Platform `orderId` |

**Note:** Use platform `orderId`, never `merchantOrderId`.

---

### 4. `payout.create` — Create a payout (withdrawal) order

**Use when:** The user wants to send crypto to an external wallet address.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |
| `merchantOrderId` | string | Your unique order ID |
| `merchantUserId` | string | Your user identifier |
| `amount` | number | Payout amount |
| `currency` | string | Currency code |
| `network` | string | Blockchain network |
| `withdrawAddress` | string | Destination wallet address |

**Key response fields to surface to user:**
- `orderId` — Platform order ID (save this!)
- `fee` — Total fee deducted
- `txId` — Transaction ID (may be pending)

**Critical notes:**
- `withdrawAddress` MUST match the selected `network`
- This does NOT replace your own approval/risk-control logic
- A successful creation response does NOT mean payout is complete — poll or use webhooks

**Common mistakes to avoid:**
- Missing `withdrawAddress` or address/network mismatch
- Treating payout as a mirror of payment (different fields, different flow)
- Including `returnUrl` or `cancelUrl` (those are payment-only)
- Assuming create success = payout complete

---

### 5. `payout.get` — Query a payout order status

**Use when:** User wants to check status of an existing payout order.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |
| `orderId` | string | Platform `orderId` |

**Key response fields:**
- `status` — Numeric status code (reference status table)
- `totalFee` / `networkFee` / `serviceFee`
- `txId` — On-chain transaction hash
- `sourceAddress` / `withdrawAddress`

---

### 6. `balance.list` — Query account balances

**Use when:** User wants to check their wallet balances.

**Required parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `environment` | string | `sandbox` or `production` |

**Response fields per currency item:**
- `currency` — Currency code
- `availableBalance` — Usable balance (for new operations)
- `lockedBalance` — Frozen/reserved balance
- `totalBalance` — Sum of both

**Important:** Use `availableBalance` for spending decisions, NOT `totalBalance`. Do not use balance query as your only concurrency control mechanism.

---

## Decision Flowchart

When user mentions payment/payout/balance:

```
User says "pay" or "收单" or "充值"
  → Is this receiving money FROM a customer?
    → YES → payment.create
    → NO  → Continue below

User says "withdraw" or "代付" or "提现" or "send"
  → Is this sending money TO an external wallet?
    → YES → payout.create
    → NO  → Ask for clarification

User says "check" or "query" or "查询" or "balance"
  → Has orderId mentioned?
    → YES → payment.get OR payout.get (determine by context)
    → NO  → balance.list

User says "cancel"
  → Has orderId mentioned?
    → YES → payment.cancel
    → NO  → Ask for orderId
```

## Error Handling

When an MCP tool returns an error:

1. **Check environment**: Is the user targeting sandbox or production?
2. **Check credentials**: Are apiKey/secretKey correctly configured?
3. **Check required fields**: Are all mandatory params present?
4. **Check field values**: Is currency valid? Does network match address?
5. **Check orderId**: Using `merchantOrderId` instead of platform `orderId`?

Common error categories:
- Auth failure → credentials issue
- Validation error → missing/wrong fields
- Not found → wrong orderId
- Insufficient balance → check with `balance.list` first
- Risk blocked → contact support

## Best Practices

### For AI Assistants

1. **Always confirm environment before action** — especially for production operations
2. **Save platform `orderId` after create** — needed for all subsequent queries
3. **Distinguish payment vs payout** — they have different fields and flows
4. **Don't assume sync completion** — crypto operations are async
5. **Surface key info proactively** — orderId, paymentUrl, fees, status

### For Integration Developers

1. Read the detailed domain docs under `docs/skill/domains/crypto/`
2. Check integration checklist at `docs/skill/references/crypto/integration-checklist.md`
3. Reference exact request/response models at `docs/skill/references/crypto/request-response-models.md`
4. Review signature examples at `docs/skill/references/shared/signature-examples.md`

## Protocol Stack Context

This Skill sits above the MCP layer in the Timar payment protocol stack:

| Layer | Protocol/Component | Role | Status |
|-------|-------------------|------|--------|
| L4 | TAP (Visa-style) | Identity & Trust | Future |
| L3 | AP2 (Google-style) | Authorization & Governance | Future |
| L2 | ACP (Stripe x OpenAI) | Discovery & Commerce | Future |
| L1 | x402 (Coinbase) | Payment Protocol Adapter | Future |
| L0 | **MCP (TimarAIMCP)** | **Capability Execution** | ✅ Done |
| — | **Skill (this file)** | **AI Trigger & Guide** | ✅ This file |

Future protocol adapters (x402, etc.) will route through MCP rather than implementing business logic independently.

## Detailed Documentation

For deeper reference, consult these documents in this repository:

### Quick Navigation
- [Integration Router](docs/skill/hub/integration-router.md) — Choose the right capability path
- [Domain Map](docs/skill/hub/domain-map.md) — All published domains overview

### Capability Details
- [Payment](docs/mcp/capabilities/payment.md) — Full payment capability spec
- [Payout](docs/mcp/capabilities/payout.md) — Full payout capability spec
- [Balance](docs/mcp/capabilities/balance.md) — Full balance capability spec
- [Notifications](docs/mcp/capabilities/notifications.md) — Webhook handling guidance

### Shared Rules
- [Auth & Signing](docs/skill/domains/shared/auth-signing.md) — Signature mechanics
- [Error Handling](docs/skill/domains/shared/error-handling.md) — Error conventions
- [Response Conventions](docs/skill/domains/shared/response-conventions.md) — Response patterns

### References
- [Endpoints](docs/skill/references/crypto/endpoints.md) — All API endpoints
- [Request/Response Models](docs/skill/references/crypto/request-response-models.md) — Exact field specs
- [Status Codes](docs/skill/references/crypto/statuses.md) — Status mappings
- [Integration Checklist](docs/skill/references/crypto/integration-checklist.md) — Pre-launch checklist
