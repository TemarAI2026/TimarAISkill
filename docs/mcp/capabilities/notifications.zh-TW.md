# Notification Capability

## Scope

本頁描述 MCP 的通知與非同步回調能力路徑。

在第一階段中，目前通知處理說明主要由數幣業務領域中的 webhook 文件承載。

## Read next

- [`../../skill/domains/crypto/handle-webhook.zh-TW.md`](../../skill/domains/crypto/handle-webhook.zh-TW.md)
- [`../../skill/domains/shared/response-conventions.zh-TW.md`](../../skill/domains/shared/response-conventions.zh-TW.md)
- [`../../skill/domains/shared/error-handling.zh-TW.md`](../../skill/domains/shared/error-handling.zh-TW.md)
- [`../../skill/references/crypto/integration-checklist.zh-TW.md`](../../skill/references/crypto/integration-checklist.zh-TW.md)

## Current phase-one expectation

- 正確接收非同步通知
- 做到冪等處理
- 在更新業務狀態時結合狀態語義與查單結果
