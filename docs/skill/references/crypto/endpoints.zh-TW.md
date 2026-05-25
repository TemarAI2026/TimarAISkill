# 加密資產端點參考

## 端點表

| 方法 | 路徑 | 用途 |
| --- | --- | --- |
| `POST` | `/api/v2/digital/payments` | 建立支付訂單。 |
| `GET` | `/api/v2/digital/payments/{orderId}` | 查詢支付訂單。 |
| `POST` | `/api/v2/digital/payments/{orderId}/cancel` | 取消支付訂單。 |
| `POST` | `/api/v2/digital/payouts` | 建立代付訂單。 |
| `GET` | `/api/v2/digital/payouts/{orderId}` | 查詢代付訂單。 |
| `GET` | `/api/v2/digital/balances` | 查詢帳戶餘額。 |
