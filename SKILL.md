# Timar Payment Skill

> **一份 SKILL，任何 AI Agent 都能用 X402 协议调用 Timar 支付能力。**

## 这是什么

本文件是一份 AI Agent 可读的技能指令。任何支持 X402 协议的 AI Agent 读取本文件后，即可通过 HTTP + 稳定币按次付费的方式调用 Timar 支付、代付和余额查询能力。

**你不需要部署任何服务，不需要 API Key，不需要注册账号。** X402 协议自动处理认证和付费。

## 架构

```
┌───────────────────────────────┐
│   任何 AI Agent               │  ← 读取这份 SKILL
│   (Claude / GPT / Cursor / …) │
└──────────────┬────────────────┘
               │ HTTP + USDC 支付 (X402 协议)
               ▼
┌───────────────────────────────┐
│   X402 适配层                  │  ← 已部署，你不需要管
│   (TimarAIMCP + X402)         │
└──────────────┬────────────────┘
               │ MCP 工具调用 + HMAC 签名
               ▼
┌───────────────────────────────┐
│   Timar 公共 API               │  ← 已部署
│   /api/v2/digital/*           │
└───────────────────────────────┘
```

**你只需要关心：向 X402 端点发 HTTP 请求，付费即可获得结果。**

## 端点一览

| 方法 | 端点 | 对应能力 | X402 支付模式 |
|------|------|---------|--------------|
| `POST` | `/v1/payment/create` | 创建支付订单（收款） | **动态** — X402 金额 = 实际转账金额，收款方 = `to` 字段 |
| `GET` | `/v1/payment/{orderId}` | 查询支付订单状态 | 无需支付，MCP API Key 鉴权 |
| `DELETE` | `/v1/payment/{orderId}` | 取消支付订单 | 无需支付，MCP API Key 鉴权 |
| `POST` | `/v1/payout/create` | 创建代付订单（付款） | **动态** — X402 金额 = 实际代付金额，收款方 = `withdrawAddress` |
| `GET` | `/v1/payout/{orderId}` | 查询代付订单状态 | 无需支付，MCP API Key 鉴权 |
| `GET` | `/v1/balance` | 查询账户余额 | 无需支付，MCP API Key 鉴权 |

> **X402 只用于转账操作：** `payment.create` 和 `payout.create` 的 X402 是实际业务转账本身，Agent 用用户授权钱包直接打 USDC 给收款方。
> 查询/取消操作由 MCP 层的 API Key 鉴权保护，直接透传，无需额外付款。

**免费端点（无需付费）：**

| 方法 | 端点 | 说明 |
|------|------|------|
| `GET` | `/health` | 服务健康检查 |
| `GET` | `/v1/tools` | 列出所有可用工具和价格 |

## 支付网络和资产

| 网络 | 链 | 支付资产 |
|------|-----|---------|
| `base` | Base (推荐，低 gas) | USDC |
| `ethereum` | Ethereum | USDC |
| `solana` | Solana | USDC (EPjFWdd5…) |

## 如何调用（X402 协议流程）

### 前提：用户授权钱包给 Agent

在调用任何支付端点前，用户需将钱包控制权（或签名权限）授权给 Agent。这是 X402 的前提条件，具体实现取决于你的 Agent 框架（如 Coinbase AgentKit、Lit Protocol 等）。

---

### `payment.create` — 两阶段 X402 流程

#### 第 1 步：发请求 → 服务器创建订单并返回 402

```http
POST /v1/payment/create HTTP/1.1
Host: {x402-server}
Content-Type: application/json

{
  "merchantOrderId": "order-001",
  "merchantUserId": "user-123",
  "amount": 100,
  "currency": "USDT",
  "network": "base"
}
```

> ⚠️ **不需要传收款地址** — 收款地址由 Timar 系统生成。

服务器内部先调 Timar API 创建订单，拿到 `receiveAddress`，然后返回 `402`：

```json
{
  "x402Version": 1,
  "accepts": [{
    "scheme": "exact",
    "network": "base",
    "maxAmountRequired": "100",
    "payTo": "0xTimarReceiveAddress",
    "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "description": "Pay 100 USDT to complete this payment (Order: pay_abc123)"
  }],
  "error": "Payment required"
}
```

> `payTo` = Timar 为本次订单生成的收款地址（来自 API 响应的 `receiveAddress` 字段）

#### 第 2 步：Agent 用用户授权钱包完成链上支付

Agent 调用钱包 SDK（如 `viem`、`ethers.js`、Coinbase SDK）：
- 向 `payTo` 地址转 `maxAmountRequired` USDC
- 获取链上交易凭证（tx hash / proof）
- 构造 `X-PAYMENT` header

#### 第 3 步：携带支付凭证重发请求

```http
POST /v1/payment/create HTTP/1.1
Host: {x402-server}
Content-Type: application/json
X-PAYMENT: {base64-encoded-payment-proof}

{
  "merchantOrderId": "order-001",
  "merchantUserId": "user-123",
  "amount": 100,
  "currency": "USDT",
  "network": "base"
}
```

#### 第 4 步：收到业务结果

服务器验证链上凭证 → 调用 Timar API → 返回 200：

```json
{
  "ok": true,
  "data": {
    "orderId": "pay_abc123",
    "status": "SUCCESS",
    "txId": "0x链上交易哈希",
    "amount": 100,
    "currency": "USDT"
  }
}
```

---

### 查询/取消端点 — 直接调用（无需 X402）

这些端点（`GET /payment/:id`、`DELETE /payment/:id`、`GET /payout/:id`、`GET /balance`）**无需付款**，由 MCP 层 API Key 鉴权。直接发请求即可：

```http
GET /v1/payment/pay_abc123 HTTP/1.1
Host: {x402-server}
```

服务器直接转发到 MCP 并返回结果，无需 402 握手，也不需要钱包。

> **如果你的 Agent 框架已内置 X402 支持**（如 Coinbase AgentKit），当服务器直接返回 200 时，框架会自动跳过支付握手。

---

## 工具详细说明

### 1. `POST /v1/payment/create` — 创建支付订单（收款）

**场景：** 用户要从客户那里收加密货币。

**请求体：**

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `merchantOrderId` | string | ✅ | 你的唯一订单 ID |
| `merchantUserId` | string | ✅ | 你的用户标识 |
| `amount` | number | ✅ | 金额 |
| `currency` | string | ✅ | 币种，如 `USDT`、`USDC` |
| `network` | string | ✅ | 区块链网络，如 `ethereum`、`tron`、`base` |
| `returnUrl` | string | ❌ | 支付完成后跳转 URL |
| `cancelUrl` | string | ❌ | 取消支付时跳转 URL |

**关键字段解读：**
- `orderId` — 平台订单号（**务必保存，后续查询/取消都用它**）
- `paymentUrl` — 给客户的支付页面链接
- `receiveAddress` — 充值地址
- `expiresInSeconds` — 过期倒计时

**常见错误：**
- ❌ 不要编造 `callbackUrl` 字段
- ❌ 不要混淆 `merchantOrderId`（你的）和 `orderId`（平台的）
- ❌ 不要把 `currency` 和 `network` 写成一个字段
- ❌ 不要展示已过期的 `paymentUrl`（先检查 `expiresInSeconds`）

---

### 2. `GET /v1/payment/{orderId}` — 查询支付状态

**场景：** 用户想查某个支付订单的状态。

**路径参数：**
- `orderId` — 平台订单号（**不是** merchantOrderId）

**查询参数：**

| 字段 | 说明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可选） |

**关键字段解读：**
- `status`: `PENDING` → 待支付 | `SUCCESS` → 已完成 | `CANCEL` → 已取消 | `RISK` → 风控拦截
- `paidAmount` — 实际支付金额
- `fee` / `feeCurrency` — 手续费
- `depositDetails` — 链上充值信息

---

### 3. `DELETE /v1/payment/{orderId}` — 取消支付订单

**场景：** 用户要取消一个未支付的订单。

**路径参数：**
- `orderId` — 平台订单号

**查询参数：**

| 字段 | 说明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可选） |

**注意：** 只能取消状态为 `PENDING` 的订单。

---

### 4. `POST /v1/payout/create` — 创建代付订单（付款到外部钱包）

**场景：** 用户要把加密货币打到外部钱包地址。

**请求体：**

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `merchantOrderId` | string | ✅ | 你的唯一订单 ID |
| `merchantUserId` | string | ✅ | 你的用户标识 |
| `amount` | number | ✅ | 金额 |
| `currency` | string | ✅ | 币种 |
| `network` | string | ✅ | 区块链网络 |
| `withdrawAddress` | string | ✅ | 目标钱包地址 |

**关键字段解读：**
- `orderId` — 平台订单号（**务必保存**）
- `fee` — 扣除的总手续费
- `txId` — 链上交易哈希（可能还在处理中）

**常见错误：**
- ❌ `withdrawAddress` 必须和 `network` 匹配（Ethereum 地址不能选 Tron 网络）
- ❌ 不要把 payout 当作 payment 的镜像（字段不同、流程不同）
- ❌ 不要包含 `returnUrl` / `cancelUrl`（这是 payment 才有的）
- ❌ **创建成功 ≠ 代付完成**，需要查询状态确认到账

---

### 5. `GET /v1/payout/{orderId}` — 查询代付状态

**场景：** 用户想查某个代付订单的状态。

**路径参数：**
- `orderId` — 平台订单号

**查询参数：**

| 字段 | 说明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可选） |

**关键字段解读：**
- `status` — 数值状态码（参考状态表）
- `totalFee` / `networkFee` / `serviceFee` — 费用明细
- `txId` — 链上交易哈希
- `sourceAddress` / `withdrawAddress` — 源地址 / 目标地址

---

### 6. `GET /v1/balance` — 查询账户余额

**场景：** 用户想看自己的钱包余额。

**查询参数：**

| 字段 | 说明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可选） |

**响应字段（每个币种一条）：**
- `currency` — 币种
- `availableBalance` — 可用余额（**用于判断能否操作**）
- `lockedBalance` — 冻结余额
- `totalBalance` — 总余额

**注意：** 用 `availableBalance` 做支出判断，**不要**用 `totalBalance`。

---

## 决策流程图

```
用户提到 "收款" / "收单" / "充值" / "pay" / "receive"
  → 是从客户那里收钱吗？
    → 是 → POST /v1/payment/create
    → 不是 → 继续判断

用户提到 "付款" / "代付" / "提现" / "withdraw" / "send"
  → 是往外部钱包打钱吗？
    → 是 → POST /v1/payout/create
    → 不是 → 询问澄清

用户提到 "查询" / "状态" / "check" / "query"
  → 有 orderId 吗？
    → 有 → 是支付还是代付？→ GET /v1/payment/{orderId} 或 GET /v1/payout/{orderId}
    → 没有 → GET /v1/balance

用户提到 "取消" / "cancel"
  → 有 orderId 吗？
    → 有 → DELETE /v1/payment/{orderId}
    → 没有 → 询问 orderId
```

## 错误处理

当请求返回错误时，按以下顺序排查：

1. **402 支付失败** → 检查钱包余额、网络是否正确
2. **认证失败** → X402 支付凭证无效，重新获取
3. **参数错误** → 检查必填字段是否完整
4. **字段值错误** → currency 是否有效？network 和地址是否匹配？
5. **orderId 不存在** → 确认使用的是平台 orderId 而非 merchantOrderId
6. **余额不足** → 先用 GET /v1/balance 查询可用余额
7. **风控拦截** → 状态为 RISK，需联系支持

## 最佳实践

1. **创建后务必保存 `orderId`** — 后续所有操作都靠它
2. **区分 payment 和 payout** — 收款用 payment，付款用 payout，字段和流程完全不同
3. **不要假设同步完成** — 加密货币操作是异步的，创建成功后需要轮询或使用 webhook
4. **主动展示关键信息** — orderId、paymentUrl、手续费、状态变化
5. **先查余额再操作** — 发起 payout 前先确认 availableBalance 足够
6. **payout 地址必须匹配网络** — Ethereum 地址选 ethereum 网络，Tron 地址选 tron 网络

## 协议栈

本 Skill 在 Timar 支付协议栈中的位置：

| 层级 | 协议/组件 | 角色 | 状态 |
|------|----------|------|------|
| L4 | TAP (Visa-style) | 身份与信任 | 规划中 |
| L3 | AP2 (Google-style) | 授权与治理 | 规划中 |
| L2 | ACP (Stripe × OpenAI) | 发现与商务 | 规划中 |
| L1 | **X402 (Coinbase)** | **支付协议适配** | ✅ 已实现 |
| L0 | **MCP (TimarAIMCP)** | **能力执行** | ✅ 已实现 |
| — | **Skill (本文件)** | **AI 触发与引导** | ✅ 本文件 |

## 详细参考文档

如需更深入的技术细节，请参考以下文档：

### 快速导航
- [集成路由](docs/skill/hub/integration-router.md) — 选择正确的能力路径
- [域地图](docs/skill/hub/domain-map.md) — 所有已发布能力域总览

### 能力详情
- [支付](docs/mcp/capabilities/payment.md) — 支付能力完整规格
- [代付](docs/mcp/capabilities/payout.md) — 代付能力完整规格
- [余额](docs/mcp/capabilities/balance.md) — 余额能力完整规格
- [通知](docs/mcp/capabilities/notifications.md) — Webhook 处理指引

### 共享规则
- [认证与签名](docs/skill/domains/shared/auth-signing.md) — 签名机制
- [错误处理](docs/skill/domains/shared/error-handling.md) — 错误规范
- [响应约定](docs/skill/domains/shared/response-conventions.md) — 响应模式

### 参考资料与示例
- [端点列表](docs/skill/references/crypto/endpoints.md) — 所有 API 端点
- [请求/响应模型](docs/skill/references/crypto/request-response-models.md) — 精确字段规格
- [状态码](docs/skill/references/crypto/statuses.md) — 状态映射
- [集成检查清单](docs/skill/references/crypto/integration-checklist.md) — 上线前检查
- [cURL 示例](docs/skill/examples/crypto/curl/) — 命令行示例
- [Node.js 示例](docs/skill/examples/crypto/nodejs/) — Node.js 代码示例
