# Timar Payment Skill

> **一份 SKILL，任何 AI Agent 都能用 X402 協議呼叫 Timar 支付能力。**

## 這是什麼

本文件是一份 AI Agent 可讀的技能指令。任何支援 X402 協議的 AI Agent 讀取本文件後，即可透過 HTTP + 穩定幣按次付費的方式呼叫 Timar 支付、代付和餘額查詢能力。

**你不需要部署任何服務，不需要 API Key，不需要註冊帳號。** X402 協議自動處理認證和付費。

## 架構

```
┌───────────────────────────────┐
│   任何 AI Agent               │  ← 讀取這份 SKILL
│   (Claude / GPT / Cursor / …) │
└──────────────┬────────────────┘
               │ HTTP + USDC 支付 (X402 協議)
               ▼
┌───────────────────────────────┐
│   X402 適配層                  │  ← 已部署，你不需要管
│   (TimarAIMCP + X402)         │
└──────────────┬────────────────┘
               │ MCP 工具呼叫 + HMAC 簽名
               ▼
┌───────────────────────────────┐
│   Timar 公共 API               │  ← 已部署
│   /api/v2/digital/*           │
└───────────────────────────────┘
```

**你只需要關心：向 X402 端點發 HTTP 請求，付費即可獲得結果。**

## 端點一覽

| 方法 | 端點 | 對應能力 | X402 支付模式 |
|------|------|---------|--------------|
| `POST` | `/v1/payment/create` | 建立支付訂單（收款） | **動態** — X402 金額 = 實際轉帳金額，收款方 = Timar receiveAddress |
| `GET` | `/v1/payment/{orderId}` | 查詢支付訂單狀態 | 無需支付，MCP API Key 鑑權 |
| `DELETE` | `/v1/payment/{orderId}` | 取消支付訂單 | 無需支付，MCP API Key 鑑權 |
| `POST` | `/v1/payout/create` | 建立代付訂單（付款） | **動態** — X402 金額 = 實際代付金額，收款方 = `withdrawAddress` |
| `GET` | `/v1/payout/{orderId}` | 查詢代付訂單狀態 | 無需支付，MCP API Key 鑑權 |
| `GET` | `/v1/balance` | 查詢帳戶餘額 | 無需支付，MCP API Key 鑑權 |

> **X402 只用於轉帳操作：** `payment.create` 和 `payout.create` 的 X402 是實際業務轉帳本身，Agent 用使用者授權錢包直接打 USDC 給收款方。
> 查詢/取消操作由 MCP 層的 API Key 鑑權保護，直接透傳，無需額外付款。

**免費端點（無需付費）：**

| 方法 | 端點 | 說明 |
|------|------|------|
| `GET` | `/health` | 服務健康檢查 |
| `GET` | `/v1/tools` | 列出所有可用工具和價格 |

## 支付網路和資產

| 網路 | 鏈 | 支付資產 |
|------|-----|---------|
| `base` | Base（建議，低 gas） | USDC |
| `ethereum` | Ethereum | USDC |
| `solana` | Solana | USDC (EPjFWdd5…) |

## 如何呼叫（X402 協議流程）

### 前提：使用者授權錢包給 Agent

在呼叫任何支付端點前，使用者需將錢包控制權（或簽名權限）授權給 Agent。這是 X402 的前提條件，具體實現取決於你的 Agent 框架（如 Coinbase AgentKit、Lit Protocol 等）。

---

### `payment.create` — 兩階段 X402 流程

#### 第 1 步：發請求 → 伺服器建立訂單並回傳 402

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

> ⚠️ **不需要傳收款地址** — 收款地址由 Timar 系統生成。

伺服器內部先呼叫 Timar API 建立訂單，取得 `receiveAddress`，然後回傳 `402`：

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

> `payTo` = Timar 為本次訂單生成的收款地址（來自 API 回應的 `receiveAddress` 欄位）

#### 第 2 步：Agent 用使用者授權錢包完成鏈上支付

Agent 呼叫錢包 SDK（如 `viem`、`ethers.js`、Coinbase SDK）：
- 向 `payTo` 地址轉 `maxAmountRequired` USDC
- 取得鏈上交易憑證（tx hash / proof）
- 建構 `X-PAYMENT` header

#### 第 3 步：攜帶支付憑證重發請求

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

#### 第 4 步：收到業務結果

伺服器驗證鏈上憑證 → 呼叫 Timar API → 回傳 200：

```json
{
  "ok": true,
  "data": {
    "orderId": "pay_abc123",
    "status": "SUCCESS",
    "txId": "0x鏈上交易雜湊",
    "amount": 100,
    "currency": "USDT"
  }
}
```

---

### 查詢/取消端點 — 直接呼叫（無需 X402）

這些端點（`GET /payment/:id`、`DELETE /payment/:id`、`GET /payout/:id`、`GET /balance`）**無需付款**，由 MCP 層 API Key 鑑權。直接發請求即可：

```http
GET /v1/payment/pay_abc123 HTTP/1.1
Host: {x402-server}
```

伺服器直接轉發到 MCP 並回傳結果，無需 402 握手，也不需要錢包。

> **如果你的 Agent 框架已內建 X402 支援**（如 Coinbase AgentKit），當伺服器直接回傳 200 時，框架會自動跳過支付握手。

---

## 工具詳細說明

### 1. `POST /v1/payment/create` — 建立支付訂單（收款）

**場景：** 使用者要從客戶那裡收加密貨幣。

**請求體：**

| 欄位 | 類型 | 必填 | 說明 |
|------|------|------|------|
| `merchantOrderId` | string | ✅ | 你的唯一訂單 ID |
| `merchantUserId` | string | ✅ | 你的使用者識別 |
| `amount` | number | ✅ | 金額 |
| `currency` | string | ✅ | 幣種，如 `USDT`、`USDC` |
| `network` | string | ✅ | 區塊鏈網路，如 `ethereum`、`tron`、`base` |
| `returnUrl` | string | ❌ | 支付完成後跳轉 URL |
| `cancelUrl` | string | ❌ | 取消支付時跳轉 URL |

**關鍵欄位解讀：**
- `orderId` — 平台訂單號（**務必儲存，後續查詢/取消都用它**）
- `paymentUrl` — 給客戶的支付頁面連結
- `receiveAddress` — 充值地址
- `expiresInSeconds` — 過期倒數計時

**常見錯誤：**
- ❌ 不要編造 `callbackUrl` 欄位
- ❌ 不要混淆 `merchantOrderId`（你的）和 `orderId`（平台的）
- ❌ 不要把 `currency` 和 `network` 寫成一個欄位
- ❌ 不要展示已過期的 `paymentUrl`（先檢查 `expiresInSeconds`）

---

### 2. `GET /v1/payment/{orderId}` — 查詢支付狀態

**場景：** 使用者想查某個支付訂單的狀態。

**路徑參數：**
- `orderId` — 平台訂單號（**不是** merchantOrderId）

**查詢參數：**

| 欄位 | 說明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可選） |

**關鍵欄位解讀：**
- `status`: `PENDING` → 待支付 | `SUCCESS` → 已完成 | `CANCEL` → 已取消 | `RISK` → 風控攔截
- `paidAmount` — 實際支付金額
- `fee` / `feeCurrency` — 手續費
- `depositDetails` — 鏈上充值資訊

---

### 3. `DELETE /v1/payment/{orderId}` — 取消支付訂單

**場景：** 使用者要取消一個未支付的訂單。

**路徑參數：**
- `orderId` — 平台訂單號

**查詢參數：**

| 欄位 | 說明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可選） |

**注意：** 只能取消狀態為 `PENDING` 的訂單。

---

### 4. `POST /v1/payout/create` — 建立代付訂單（付款到外部錢包）

**場景：** 使用者要把加密貨幣打到外部錢包地址。

**請求體：**

| 欄位 | 類型 | 必填 | 說明 |
|------|------|------|------|
| `merchantOrderId` | string | ✅ | 你的唯一訂單 ID |
| `merchantUserId` | string | ✅ | 你的使用者識別 |
| `amount` | number | ✅ | 金額 |
| `currency` | string | ✅ | 幣種 |
| `network` | string | ✅ | 區塊鏈網路 |
| `withdrawAddress` | string | ✅ | 目標錢包地址 |

**關鍵欄位解讀：**
- `orderId` — 平台訂單號（**務必儲存**）
- `fee` — 扣除的總手續費
- `txId` — 鏈上交易雜湊（可能還在處理中）

**常見錯誤：**
- ❌ `withdrawAddress` 必須和 `network` 匹配（Ethereum 地址不能選 Tron 網路）
- ❌ 不要把 payout 當作 payment 的鏡像（欄位不同、流程不同）
- ❌ 不要包含 `returnUrl` / `cancelUrl`（這是 payment 才有的）
- ❌ **建立成功 ≠ 代付完成**，需要查詢狀態確認到帳

---

### 5. `GET /v1/payout/{orderId}` — 查詢代付狀態

**場景：** 使用者想查某個代付訂單的狀態。

**路徑參數：**
- `orderId` — 平台訂單號

**查詢參數：**

| 欄位 | 說明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可選） |

**關鍵欄位解讀：**
- `status` — 數值狀態碼（參考狀態表）
- `totalFee` / `networkFee` / `serviceFee` — 費用明細
- `txId` — 鏈上交易雜湊
- `sourceAddress` / `withdrawAddress` — 來源地址 / 目標地址

---

### 6. `GET /v1/balance` — 查詢帳戶餘額

**場景：** 使用者想看自己的錢包餘額。

**查詢參數：**

| 欄位 | 說明 |
|------|------|
| `environment` | `sandbox` 或 `production`（可選） |

**回應欄位（每個幣種一條）：**
- `currency` — 幣種
- `availableBalance` — 可用餘額（**用於判斷能否操作**）
- `lockedBalance` — 凍結餘額
- `totalBalance` — 總餘額

**注意：** 用 `availableBalance` 做支出判斷，**不要**用 `totalBalance`。

---

## 決策流程圖

```
使用者提到「收款」/「收單」/「儲值」/「pay」/「receive」
  → 是從客戶那裡收錢嗎？
    → 是 → POST /v1/payment/create
    → 不是 → 繼續判斷

使用者提到「付款」/「代付」/「提現」/「withdraw」/「send」
  → 是往外部錢包打錢嗎？
    → 是 → POST /v1/payout/create
    → 不是 → 詢問澄清

使用者提到「查詢」/「狀態」/「check」/「query」
  → 有 orderId 嗎？
    → 有 → 是支付還是代付？→ GET /v1/payment/{orderId} 或 GET /v1/payout/{orderId}
    → 沒有 → GET /v1/balance

使用者提到「取消」/「cancel」
  → 有 orderId 嗎？
    → 有 → DELETE /v1/payment/{orderId}
    → 沒有 → 詢問 orderId
```

## 錯誤處理

當請求回傳錯誤時，按以下順序排查：

1. **402 支付失敗** → 檢查錢包餘額、網路是否正確
2. **認證失敗** → X402 支付憑證無效，重新取得
3. **參數錯誤** → 檢查必填欄位是否完整
4. **欄位值錯誤** → currency 是否有效？network 和地址是否匹配？
5. **orderId 不存在** → 確認使用的是平台 orderId 而非 merchantOrderId
6. **餘額不足** → 先用 GET /v1/balance 查詢可用餘額
7. **風控攔截** → 狀態為 RISK，需聯絡支援

## 最佳實踐

1. **建立後務必儲存 `orderId`** — 後續所有操作都靠它
2. **區分 payment 和 payout** — 收款用 payment，付款用 payout，欄位和流程完全不同
3. **不要假設同步完成** — 加密貨幣操作是非同步的，建立成功後需要輪詢或使用 webhook
4. **主動展示關鍵資訊** — orderId、paymentUrl、手續費、狀態變化
5. **先查餘額再操作** — 發起 payout 前先確認 availableBalance 足夠
6. **payout 地址必須匹配網路** — Ethereum 地址選 ethereum 網路，Tron 地址選 tron 網路

## 協議棧

本 Skill 在 Timar 支付協議棧中的位置：

| 層級 | 協議/組件 | 角色 | 狀態 |
|------|----------|------|------|
| L4 | TAP (Visa-style) | 身份與信任 | 規劃中 |
| L3 | AP2 (Google-style) | 授權與治理 | 規劃中 |
| L2 | ACP (Stripe × OpenAI) | 發現與商務 | 規劃中 |
| L1 | **X402 (Coinbase)** | **支付協議適配** | ✅ 已實現 |
| L0 | **MCP (TimarAIMCP)** | **能力執行** | ✅ 已實現 |
| — | **Skill (本文件)** | **AI 觸發與引導** | ✅ 本文件 |

## 詳細參考文件

如需更深入的技術細節，請參考以下文件：

### 快速導航
- [整合路由](docs/skill/hub/integration-router.md) — 選擇正確的能力路徑
- [域地圖](docs/skill/hub/domain-map.md) — 所有已發布能力域總覽

### 能力詳情
- [支付](docs/mcp/capabilities/payment.md) — 支付能力完整規格
- [代付](docs/mcp/capabilities/payout.md) — 代付能力完整規格
- [餘額](docs/mcp/capabilities/balance.md) — 餘額能力完整規格
- [通知](docs/mcp/capabilities/notifications.md) — Webhook 處理指引

### 共享規則
- [認證與簽名](docs/skill/domains/shared/auth-signing.md) — 簽名機制
- [錯誤處理](docs/skill/domains/shared/error-handling.md) — 錯誤規範
- [回應約定](docs/skill/domains/shared/response-conventions.md) — 回應模式

### 參考資料與範例
- [端點列表](docs/skill/references/crypto/endpoints.md) — 所有 API 端點
- [請求/回應模型](docs/skill/references/crypto/request-response-models.md) — 精確欄位規格
- [狀態碼](docs/skill/references/crypto/statuses.md) — 狀態對映
- [整合檢查清單](docs/skill/references/crypto/integration-checklist.md) — 上線前檢查
- [cURL 範例](docs/skill/examples/crypto/curl/) — 命令列範例
- [Node.js 範例](docs/skill/examples/crypto/nodejs/) — Node.js 程式碼範例
