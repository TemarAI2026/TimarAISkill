# Crypto Statuses Reference

## Payment Order Status Table

| Source value | Enum member | Current normalized meaning |
| --- | --- | --- |
| `0` | `None` | `PENDING` |
| `4` | `Completed` | `SUCCESS` |
| `5` | `ManualCompleted` | `SUCCESS` |
| `6` | `Closed` | `CANCEL` |
| `7` | `Risk` | `RISK` |

## Payout Status Table

| Source value | Enum member | Current normalized meaning |
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

## Notes

- These are the current source-backed values and may expand in future releases.
- Downstream systems should retain the raw source value and handle unknown future values safely with logging.
- Payment query status is backed by `OrderStatus` in source logic, but the public `data.status` field is serialized on the wire as its normalized string value such as `PENDING`, `SUCCESS`, `CANCEL`, or `RISK`.
- Payout query status is currently returned on the public wire as a raw `int` value; map it with the payout table above.
