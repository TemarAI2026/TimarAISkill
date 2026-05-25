# 請求標頭參考

## 標頭表

| Header | 必填 | 說明 |
| --- | --- | --- |
| `X-Api-Key` | 是 | 商戶 API key。 |
| `X-Api-Timestamp` | 是 | 請求時間，支援 Unix 毫秒時間戳或 ISO8601。 |
| `X-Api-RequestId` | 是 | 請求唯一識別碼，用於追蹤、排障與短期去重。 |
| `X-Api-Sign` | 是 | 依目前簽名規則計算出的 Base64 簽名值。 |

## 說明

- 每次請求都應產生新的 `X-Api-RequestId`。
- 有請求本文時，簽名原文是 `timestamp + requestId + rawBody`。
- 無請求本文時，簽名原文是 `timestamp + requestId`。
- 目前已知的公開錯誤訊息包含 `Timestamp expired.`，通常應先檢查時間戳格式或時效視窗。
- 公開文件建議對 `X-Api-RequestId` 做短期去重，以降低重放風險。
