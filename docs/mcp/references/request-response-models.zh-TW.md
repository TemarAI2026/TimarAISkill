# 請求與回應模型

## 用途

本頁彙總目前 MCP-facing 的收單、代付、餘額客戶端實作時最需要關注的公開請求與回應模型形態。

## 公共頂層回應包裝

目前所有公開回應都使用同一個頂層包裝：

| 欄位 | 含義 |
| --- | --- |
| `code` | 業務結果碼。成功固定為 `"0"`。 |
| `msg` | 可讀結果或錯誤訊息。 |
| `data` | 具體業務載荷，形態取決於端點。 |

## Payment create

目前請求欄位：

- 必填：`merchantOrderId`、`merchantUserId`、`amount`、`currency`、`network`
- 可選：`returnUrl`、`cancelUrl`

目前建議保留的回應欄位：

- `orderId`
- `merchantOrderId`
- `paymentUrl`
- `receiveAddress`
- `amount`
- `currency`
- `network`
- `expiresInSeconds`

## Payment query

目前常用回應欄位：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `status`
- `paidAmount`
- `fee`
- `feeCurrency`
- `depositDetails`

## Payout create

目前請求欄位：

- 必填：`merchantOrderId`、`merchantUserId`、`amount`、`currency`、`network`、`withdrawAddress`

目前建議保留的回應欄位：

- `orderId`
- `merchantOrderId`
- `merchantUserId`
- `amount`
- `fee`
- `currency`
- `network`
- `withdrawAddress`

## Payout query

目前常用回應欄位：

- `orderId`
- `merchantOrderId`
- `status`
- `totalFee`
- `networkFee`
- `serviceFee`
- `txId`
- `sourceAddress`
- `feeCurrency`

## Balance query

目前回應項欄位：

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

## 常見錯誤

- 只處理 `data`，忽略 `code` 和 `msg`
- 在目前未發布的 create 請求裡臆造 `callbackUrl`
- 只保留 `merchantOrderId`，丟掉平台 `orderId`
- 把餘額欄位當成訂單欄位來處理

## 精確來源參考

- [`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)
