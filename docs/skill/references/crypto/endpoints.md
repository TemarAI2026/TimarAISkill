# 加密资产端点参考

## 端点表

| 方法 | 路径 | 用途 |
| --- | --- | --- |
| `POST` | `/api/v2/digital/payments` | 创建支付订单。 |
| `GET` | `/api/v2/digital/payments/{orderId}` | 查询支付订单。 |
| `POST` | `/api/v2/digital/payments/{orderId}/cancel` | 取消支付订单。 |
| `POST` | `/api/v2/digital/payouts` | 创建代付订单。 |
| `GET` | `/api/v2/digital/payouts/{orderId}` | 查询代付订单。 |
| `GET` | `/api/v2/digital/balances` | 查询账户余额。 |
