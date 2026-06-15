# 通知能力

## 适用范围

第一阶段 MCP 的 notification capability 覆盖的是异步订单通知的接收侧处理。它的目标不是臆造一套 webhook 协议，而是约束接入方和 AI 助手如何围绕已发布契约实现一个安全的接收器。

## 第一阶段当前要求

- 正确接收异步通知
- 做好幂等处理
- 将通知数据与本地订单记录做核对
- 在需要时结合查单结果与当前状态语义完成业务更新

## 实现前先确认什么

在实现通知处理前，应先确认当前已发布集成契约中的这些部分：

- 发送方身份
- 投递路径
- 请求头
- 签名或验签规则
- 负载结构
- 重试行为
- 成功响应要求

如果这些点没有被清晰发布，就不要在 MCP 层自行猜测。

## 接收器实现说明

- 记录稳定的请求追踪标识
- 保留原始请求体和关键请求头，便于排查
- 围绕稳定事件键，或“订单 + 状态流转”设计幂等逻辑
- 更新本地状态前，先把订单号、金额、币种、网络等关键值与自身记录做核对
- 只有在公开契约明确说明时，才把通知载荷直接视为最终权威

## 常见错误

- 在公开契约未定义时，自行发明 webhook 字段或验签规则
- 对重复通知进行多次业务处理
- 在未核对通知值前，直接更新本地业务状态
- 在业务规则仍要求补充查单时，仅凭通知成功就认为证据已经足够

## 精确参考

- [`../../skill/domains/crypto/handle-webhook.md`](../../skill/domains/crypto/handle-webhook.md)
- [`../../skill/domains/shared/response-conventions.md`](../../skill/domains/shared/response-conventions.md)
- [`../../skill/domains/shared/error-handling.md`](../../skill/domains/shared/error-handling.md)
- [`../../skill/references/crypto/integration-checklist.md`](../../skill/references/crypto/integration-checklist.md)
