# 数币状态参考

## 支付订单状态表

| 源值 | 枚举成员 | 当前归一含义 |
| --- | --- | --- |
| `0` | `None` | `PENDING` |
| `4` | `Completed` | `SUCCESS` |
| `5` | `ManualCompleted` | `SUCCESS` |
| `6` | `Closed` | `CANCEL` |
| `7` | `Risk` | `RISK` |

## 代付状态表

| 源值 | 枚举成员 | 当前归一含义 |
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

## 说明

- 以上是当前源码确认的状态值，后续版本可能继续扩展。
- 下游系统应保留原始源值，并对未知新值做安全兜底和日志记录。
- 支付查询的状态在源码逻辑中背靠 `OrderStatus`，但对外 `data.status` 在线上会序列化为 `PENDING`、`SUCCESS`、`CANCEL`、`RISK` 等归一后的字符串值。
- 代付查询的状态当前在对外线上仍然返回原始 `int` 值，应按上表做映射。
