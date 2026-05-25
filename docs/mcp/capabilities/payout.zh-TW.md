# 代付能力

## 適用範圍

第一階段 MCP 的 payout capability 目前覆蓋的是數幣代付訂單相關操作。當前已發布的操作集合包括：

- 建立代付訂單
- 查詢代付訂單

## 目前介面族

- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`

## 呼叫前需要準備什麼

呼叫此能力前，應先具備以下前提：

- 商戶入駐與帳戶配置已完成
- 執行環境、`apiKey`、`secretKey` 與簽名邏輯已配置好
- 業務系統能產生唯一的 `merchantOrderId`
- 業務系統已明確 `merchantUserId`、`amount`、`currency`、`network` 與 `withdrawAddress`
- 餘額校驗、提交權限與業務側放行策略，應先由你自己的業務層決定

這一層 MCP 不能取代你自己的審批、風控或資金預留邏輯。

## 建立訂單

使用 `POST /api/v2/digital/payouts` 建立新的代付訂單。

目前請求概念包括：

- 必填：`merchantOrderId`
- 必填：`merchantUserId`
- 必填：`amount`
- 必填：`currency`
- 必填：`network`
- 必填：`withdrawAddress`

目前建立回應中，呼叫方通常需要持久化或消費這些欄位：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## 查詢訂單

使用 `GET /api/v2/digital/payouts/{orderId}` 取得代付目前狀態。

目前查詢回應會暴露這類關鍵資訊：

- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

請同時保存平台 `orderId` 與你自己的 `merchantOrderId`，否則後續查單與對帳會受影響。

## 狀態跟進說明

目前 payout query 的狀態在公開回應中以原始 `int` 值返回。應依照已發布的 payout 狀態表做映射，並為未來未知狀態保留安全處理邏輯。

不要把「建立成功返回」直接當成「代付最終完成」。業務側通常仍需要輪詢或結合異步通知持續確認。

## 常見錯誤

- 漏傳 `withdrawAddress`，或地址與 `network` 不匹配
- 把 payout 當成 payment 的鏡像流程
- 混入 `returnUrl`、`cancelUrl`、`callbackUrl` 這類 payment 才有的欄位
- 只保存 `merchantOrderId`，丟掉平台 `orderId`
- 把首次建立回應直接當成最終完成狀態

## 精確參考

- [`../../skill/domains/crypto/overview.zh-TW.md`](../../skill/domains/crypto/overview.zh-TW.md)
- [`../../skill/domains/crypto/create-payout.zh-TW.md`](../../skill/domains/crypto/create-payout.zh-TW.md)
- [`../../skill/domains/crypto/query-order.zh-TW.md`](../../skill/domains/crypto/query-order.zh-TW.md)
- [`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
- [`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)
- [`../../skill/references/crypto/statuses.zh-TW.md`](../../skill/references/crypto/statuses.zh-TW.md)
