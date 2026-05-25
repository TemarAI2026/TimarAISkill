# 端點參考

## 用途

本頁列出第一階段數幣能力集中目前已發布的 MCP-facing 端點族。

## 目前已發布端點

| Method | Path | 能力 | 用途 |
| --- | --- | --- | --- |
| `POST` | `/api/v2/digital/payments` | `payment` | 建立收單訂單。 |
| `GET` | `/api/v2/digital/payments/{orderId}` | `payment` | 查詢收單訂單。 |
| `POST` | `/api/v2/digital/payments/{orderId}/cancel` | `payment` | 取消收單訂單。 |
| `POST` | `/api/v2/digital/payouts` | `payout` | 建立代付訂單。 |
| `GET` | `/api/v2/digital/payouts/{orderId}` | `payout` | 查詢代付訂單。 |
| `GET` | `/api/v2/digital/balances` | `balance` | 查詢帳戶餘額。 |

## 使用說明

- payment 與 payout 不是共用同一個 create 端點
- 目前查單路徑使用的是平台 `orderId` 路徑參數
- 目前公開 cancel 只定義在 payment 訂單上
- 餘額查詢是帳戶視圖端點，不是訂單端點

## 常見錯誤

- 用 payout 的請求欄位去呼叫 payment 路徑
- 明明公開路徑要求平台 `orderId`，卻用 `merchantOrderId` 去查
- 因為 payment 有 cancel，就預設 payout 也有公開 cancel
- 把餘額查詢當成訂單狀態查詢使用

## 精確來源參考

- [`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
