# 收單能力

## 適用範圍

第一階段 MCP 的 payment capability 目前覆蓋的是數幣收單訂單相關操作。當前已發布的操作集合包括：

- 建立收單訂單
- 查詢收單訂單
- 取消收單訂單

## 目前介面族

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`

## 呼叫前需要準備什麼

呼叫此能力前，應先具備以下前提：

- 商戶入駐與帳戶配置已完成
- 執行環境、`apiKey`、`secretKey` 與簽名邏輯已配置好
- 業務系統能產生唯一的 `merchantOrderId`
- 業務系統已明確 `merchantUserId`、`amount`、`currency` 與 `network`

這一層 MCP 預設商戶身份與上游業務上下文都已準備完成。

## 建立訂單

使用 `POST /api/v2/digital/payments` 建立新的收單訂單。

目前請求概念包括：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 可選：`returnUrl`
- 可選：`cancelUrl`

目前建立回應中，呼叫方通常需要持久化或消費這些欄位：

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## 查詢與取消

使用 `GET /api/v2/digital/payments/{orderId}` 取得訂單目前狀態。

目前查詢回應會暴露這類關鍵資訊：

- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

使用 `POST /api/v2/digital/payments/{orderId}/cancel` 時，只應傳入真正要取消的平台 `orderId`。不要假設 `merchantOrderId` 可直接取代 `orderId`。

## 狀態消費說明

目前 payment query 的狀態在公開回應中以規範化字串返回，典型值包括：

- `PENDING`
- `SUCCESS`
- `CANCEL`
- `RISK`

建議持久化原始回應，並為未來新增狀態值保留安全兜底邏輯。

## 常見錯誤

- 在目前建立收單請求中臆造 `callbackUrl`
- 把 `merchantOrderId` 和平台 `orderId` 當成同一個識別值
- 把 `currency` 與 `network` 混成一個欄位
- 把 `returnUrl` 或 `cancelUrl` 當成伺服器異步通知地址
- 忽略 `expiresInSeconds`，仍對外展示已過期的支付訂單

## 精確參考

- [`../../skill/domains/crypto/overview.zh-TW.md`](../../skill/domains/crypto/overview.zh-TW.md)
- [`../../skill/domains/crypto/create-payment.zh-TW.md`](../../skill/domains/crypto/create-payment.zh-TW.md)
- [`../../skill/domains/crypto/query-order.zh-TW.md`](../../skill/domains/crypto/query-order.zh-TW.md)
- [`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
- [`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)
- [`../../skill/references/crypto/statuses.zh-TW.md`](../../skill/references/crypto/statuses.zh-TW.md)
