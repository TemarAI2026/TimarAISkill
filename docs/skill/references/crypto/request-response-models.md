# 请求与响应模型参考

## 顶层响应包装

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `code` | `string` | 顶层业务码；成功固定为 `"0"`。 |
| `msg` | `string` | 顶层提示或错误消息。 |
| `data` | `object \| boolean \| array \| null` | 顶层业务数据，具体形状取决于接口。 |

- 创建支付对外返回 `CryptoApiResponse<CreateCryptoPaymentV2Response>`。
- 查询支付对外返回 `CryptoApiResponse<GetCryptoPaymentV2Response>`。
- 取消支付对外返回 `CryptoApiResponse<bool>`。
- 创建代付对外返回 `CryptoApiResponse<CreateCryptoPayoutV2Response>`。
- 查询代付对外返回 `CryptoApiResponse<GetCryptoPayoutV2Response>`。
- 查询余额对外返回 `CryptoApiResponse<List<CryptoBalanceV2Response>>`。
- 下文各响应表描述的是顶层 `data` 字段内部的当前结构。

## 创建支付请求

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | 是 | 商户订单号。 |
| `merchantUserId` | `string` | 是 | 商户侧用户标识。 |
| `amount` | `decimal` | 是 | 订单金额。 |
| `currency` | `string` | 是 | 币种。 |
| `network` | `string` | 是 | 网络名称。 |
| `returnUrl` | `string` | 否 | 支付完成后的跳转地址。 |
| `cancelUrl` | `string` | 否 | 取消支付后的跳转地址。 |

## 创建支付响应

当前对外线上的响应包装为 `CryptoApiResponse<CreateCryptoPaymentV2Response>`，下表描述 `data` 内部字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | `string` | 平台订单号。 |
| `merchantOrderId` | `string` | 商户订单号。 |
| `paymentUrl` | `string` | 支付链接。 |
| `amount` | `decimal` | 订单金额。 |
| `receiveAddress` | `string` | 收款地址。 |
| `currency` | `string` | 币种。 |
| `network` | `string` | 网络名称。 |
| `expiresInSeconds` | `int` | 支付有效期，单位为秒。 |

## 查询支付响应

当前对外线上的响应包装为 `CryptoApiResponse<GetCryptoPaymentV2Response>`，下表描述 `data` 内部字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | `string` | 平台订单号。 |
| `merchantOrderId` | `string` | 商户订单号。 |
| `merchantUserId` | `string` | 商户侧用户标识。 |
| `amount` | `decimal` | 订单金额。 |
| `status` | `string` | 当前对外线上的状态字符串，按现状序列化为 `PENDING`、`SUCCESS`、`CANCEL`、`RISK` 等环境值；不是原始枚举对象。 |
| `currency` | `string` | 币种。 |
| `network` | `string` | 网络名称。 |
| `paidAmount` | `decimal` | 已支付金额。 |
| `paymentUrl` | `string` | 支付链接。 |
| `receiveAddress` | `string` | 收款地址。 |
| `expiresInSeconds` | `int` | 支付有效期，单位为秒。 |
| `fee` | `decimal` | 手续费。 |
| `feeCurrency` | `string` | 手续费币种。 |
| `depositDetails` | `List<CryptoPaymentDepositDetailV2Response>` | 每项当前包含 `amount`、`txId`、`sourceAddress`。 |

## 创建代付请求

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | 是 | 商户订单号。 |
| `merchantUserId` | `string` | 是 | 商户侧用户标识。 |
| `amount` | `decimal` | 是 | 订单金额。 |
| `currency` | `string` | 是 | 币种。 |
| `network` | `string` | 是 | 网络名称。 |
| `withdrawAddress` | `string` | 是 | 提币地址。 |

## 创建代付响应

当前对外线上的响应包装为 `CryptoApiResponse<CreateCryptoPayoutV2Response>`，下表描述 `data` 内部字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | `string` | 平台订单号。 |
| `merchantOrderId` | `string` | 商户订单号。 |
| `merchantUserId` | `string` | 商户侧用户标识。 |
| `amount` | `decimal` | 订单金额。 |
| `fee` | `decimal` | 手续费。 |
| `currency` | `string` | 币种。 |
| `network` | `string` | 网络名称。 |
| `withdrawAddress` | `string` | 提币地址。 |

## 查询代付响应

当前对外线上的响应包装为 `CryptoApiResponse<GetCryptoPayoutV2Response>`，下表描述 `data` 内部字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | `string` | 平台订单号。 |
| `merchantOrderId` | `string` | 商户订单号。 |
| `merchantUserId` | `string` | 商户侧用户标识。 |
| `amount` | `decimal` | 订单金额。 |
| `totalFee` | `decimal` | 总手续费。 |
| `networkFee` | `decimal` | 链上手续费。 |
| `serviceFee` | `decimal` | 服务费。 |
| `status` | `int` | 当前代付状态值，见状态参考。 |
| `currency` | `string` | 币种。 |
| `network` | `string` | 网络名称。 |
| `txId` | `string` | 链上交易号。 |
| `withdrawAddress` | `string` | 提币地址。 |
| `sourceAddress` | `string` | 出款地址。 |
| `feeCurrency` | `string` | 手续费币种。 |

## 查询余额响应

当前对外线上的响应包装为 `CryptoApiResponse<List<CryptoBalanceV2Response>>`，下表描述 `data` 数组中每个元素的字段。

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `currency` | `string` | 币种。 |
| `availableBalance` | `decimal` | 可用余额。 |
| `lockedBalance` | `decimal` | 冻结余额。 |
| `totalBalance` | `decimal` | 总余额。 |
