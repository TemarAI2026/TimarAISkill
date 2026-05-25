# 错误码参考

## 常见错误码

| 外部 code | 当前含义 | 常见触发场景 | 下一步检查 |
| --- | --- | --- | --- |
| `"0"` | 成功 | 请求处理成功。 | 继续按业务流程处理响应数据。 |
| `400` | `ParameterInvalid` | 缺少请求头、时间戳格式无效、时间戳过期、参数校验失败。 | 先检查必填字段、四个请求头，以及是否出现 `Timestamp expired.`。 |
| `401` | `SignaturesNotMatch` | 签名不匹配。 | 核对 `secretKey`、原始请求体字符串、`timestamp`、`requestId` 和签名公式。 |
| `500` | `ServerError` | 服务端异常或无更具体的公开错误码。 | 记录 `X-Api-RequestId`，保留原始请求并联系支持排查。 |
| `40001` | `CoinNotExist` | 币种不存在或当前配置不支持。 | 检查 `currency` 与业务配置是否匹配。 |
| `40007` | `OrderNotFound` | 支付订单不存在。 | 核对平台返回的 `orderId` 是否正确，并确认订单是否属于当前商户。 |
| `40008` | `WithdrawalNotFound` | 代付订单不存在。 | 核对平台返回的 `orderId` 是否正确，并确认查询的是代付单。 |
| `40009` | `OutOrderNoIsExists` | 商户订单号已存在。 | 检查 `merchantOrderId` 是否被重复提交。 |
| `40012` | `WithdrawalHasBeenTurnedOff` | 代付功能当前关闭。 | 先确认账户配置和代付开通状态。 |
| `40021` | `BusinessFrozenOrInactive` | 商户被冻结或未激活。 | 检查商户状态并联系支持。 |

## 说明

- 当前 Crypto V2 对外响应里的 `code` 是字符串；成功固定返回 `"0"`。
- 同一个错误码可能对应不同公开消息；`Timestamp expired.` 是当前已知的公开消息之一。
- 排障时优先保留 `X-Api-RequestId`、原始请求体和收到的 `code`/`msg`。
