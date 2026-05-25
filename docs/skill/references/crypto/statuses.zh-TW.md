# 數幣狀態參考

## 支付訂單狀態表

| 源值 | 列舉成員 | 目前歸一含義 |
| --- | --- | --- |
| `0` | `None` | `PENDING` |
| `4` | `Completed` | `SUCCESS` |
| `5` | `ManualCompleted` | `SUCCESS` |
| `6` | `Closed` | `CANCEL` |
| `7` | `Risk` | `RISK` |

## 代付狀態表

| 源值 | 列舉成員 | 目前歸一含義 |
| --- | --- | --- |
| `0` | `PendingBusinessApproval` | `PENDING_BUSINESS_APPROVAL` |
| `1` | `PendingOperateApproval` | `PENDING` |
| `2` | `PendingRelease` | `PENDING` |
| `3` | `TransferInProgress` | `PENDING` |
| `4` | `Rejected` | `CANCEL` |
| `5` | `Completed` | `SUCCESS` |
| `6` | `PartiallyCompleted` | `PENDING` |
| `7` | `Exception` | `PENDING` |
| `8` | `SystemAuditInProgress` | `PENDING` |
| `9` | `OnChain` | `PENDING` |

## 說明

- 以上是目前由原始碼確認的狀態值，後續版本可能持續擴充。
- 下游系統應保留原始源值，並對未知新值做好安全兜底與日誌記錄。
- 支付查詢的狀態在原始碼邏輯中背靠 `OrderStatus`，但對外 `data.status` 在線上會序列化為 `PENDING`、`SUCCESS`、`CANCEL`、`RISK` 等歸一化後的字串值。
- 代付查詢的狀態目前在對外線上仍然回傳原始 `int` 值，應按上表進行映射。
