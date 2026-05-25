# 環境說明

## 用途

本頁定義第一階段 MCP 接入的執行期環境模型，目的是避免外部接入方與 AI 編程助手在不同環境之間混用憑證、混用假設、混用上線判斷。

## 執行期模型

在執行期階段，MCP 接入聚焦於少量動作：

- 選擇環境
- 載入該環境對應的憑證
- 產生簽名輸入
- 呼叫已發布能力
- 在同一環境內核對返回狀態與識別值

環境切換屬於執行期配置問題，不屬於商戶開戶問題。

## 目前環境概念

目前公開文件預設至少存在這兩類環境概念：

- `sandbox`
- `production`

實際 base URL、啟用能力、發布時間窗與操作規則，應以正式平台配置與目前精確 reference 為準。

## 最小環境配置

每個環境都應被建模為獨立配置集合，至少包括：

- base URL
- `apiKey`
- `secretKey`
- 請求逾時策略
- 日誌或追蹤策略

不要在 sandbox 與 production 之間共用一套憑證配置。

## 前置條件

在進入執行期環境接入前，應先完成：

- 商戶註冊
- 接入開通
- API 憑證簽發

MCP 執行期文件不取代這些開戶與准入步驟。

## 隔離規則

建議始終保持這些環境隔離規則：

- `apiKey` 與 `secretKey` 必須按環境分開管理
- 不要混用 sandbox 與 production 的請求參數
- 不要跨環境重用已保存的 `orderId`
- 不要因為某個能力在 sandbox 可用，就預設它在 production 也已開通
- 不要只憑 sandbox 的狀態行為就推斷 production 一定一致，必須重新驗證

## 上線前檢查

在發布到 production 前，至少要在目標環境重新驗證：

1. 簽名行為
2. 必填請求標頭
3. 端點路徑與方法
4. 狀態處理與後續跟進邏輯
5. 異步通知或回調行為，如有使用
6. 日誌與追蹤留存

## 常見錯誤

- 選對了環境，卻用了另一套 `apiKey` / `secretKey`
- 用 sandbox 的 base URL 去打 production，或反過來
- 沒有重新檢查環境差異，就把客戶端程式碼直接從測試推進到生產
- 把訂單查詢做成「無環境區分」的查找邏輯，導致跨環境誤查

## 相關頁面

- [`./architecture.zh-TW.md`](./architecture.zh-TW.md)
- [`./auth-signing.zh-TW.md`](./auth-signing.zh-TW.md)
- [`../references/common.zh-TW.md`](../references/common.zh-TW.md)
