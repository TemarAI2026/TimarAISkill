# Environments

## Runtime model

MCP 執行期接入只聚焦少數幾類動作：

- 選擇環境
- 配置憑證
- 產生簽名輸入
- 呼叫能力

## Supported environment idea

目前文件預設至少存在以下環境語義：

- `sandbox`
- `production`

實際 base URL、可用能力與發布規則，應以正式平台設定與目前已發布參考資料為準。

## Prerequisites

環境切換不是開戶流程的一部分。  
在進入執行期前，應已完成：

- 商戶註冊
- 存取開通
- API 憑證發放

## Operational guidance

- 在不同環境中分開管理 `apiKey` 與 `secretKey`
- 不要混用 sandbox 與 production 的請求參數
- 發布前先在目標環境重新驗證簽名、狀態流與回調行為
