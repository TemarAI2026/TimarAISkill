# 加密资产集成检查清单

## 开发前

- 确认环境里已安全配置 `X-Api-Key` 与 `secretKey`，且 `secretKey` 不进入日志。
- 实现四个必填请求头，以及基于原始请求体字符串的签名逻辑。
- 为每次请求生成新的 `X-Api-RequestId`，并对其做短期去重。
- 为每个创建请求生成唯一的 `merchantOrderId`。
- 持久化 `merchantOrderId`、平台返回的 `orderId`、`X-Api-RequestId`、请求时间戳和原始请求/响应。
- 将支付和代付建模为两条独立流程，不混用 `returnUrl`、`cancelUrl` 与 `withdrawAddress`。
- 先按当前状态参考设计状态归一和未知值兜底。
- 代付流程在业务侧提前规划余额检查与失败处理。

## 上线前

- 逐项核对当前配置的 6 个公开端点路径与方法完全一致。
- 分别验证有请求体和无请求体的签名，并覆盖 Unix 毫秒与 ISO8601 两种时间戳格式。
- 验证缺少请求头、签名错误、`Timestamp expired.`、订单不存在、商户订单号重复等场景的日志和告警。
- 验证创建支付后已保存 `paymentUrl`、`receiveAddress`、`expiresInSeconds` 和平台 `orderId`。
- 验证支付查询与取消流程会先查单，再按 `orderId` 调用取消接口。
- 验证创建代付后会保存 `fee`、`withdrawAddress`，查单后会更新 `totalFee`、`networkFee`、`serviceFee`、`txId`、`sourceAddress`。
- 验证余额处理清楚区分 `availableBalance`、`lockedBalance`、`totalBalance`。
- 验证重试策略不会盲目重发创建请求，并且能用已保存的 `X-Api-RequestId` 和 `orderId` 做排障。
