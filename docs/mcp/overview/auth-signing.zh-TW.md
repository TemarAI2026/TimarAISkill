# 鑑權與簽名

## 適用範圍

本頁定義第一階段 MCP 執行期請求鑑權的最小公開契約。無論是人工接入方或 AI 編程助手，在呼叫任何已發布 API 能力前，都應先符合這裡的規則。

## 每次請求都要準備的執行期輸入

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

不要重複使用舊的 `requestId`、時間戳或簽名。

## 必填請求標頭

目前公開呼叫要求完整帶上以下四個標頭：

- `X-Api-Key`
- `X-Api-Timestamp`
- `X-Api-RequestId`
- `X-Api-Sign`

`X-Api-Timestamp` 目前接受 Unix 毫秒時間戳或 ISO8601。已知公開錯誤之一是 `Timestamp expired.`，因此客戶端時間需要和服務端保持對齊，且不要重放過期請求。

## 簽名載荷規則

- 有請求體時：`timestamp + requestId + rawBody`
- 無請求體時：`timestamp + requestId`

這裡的 `rawBody` 指真正送上線路的原始字串。應該對最終序列化結果簽名，而不是對重新組裝的物件內容簽名。

## 簽名公式

```text
Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))
```

## 執行期檢查清單

送出請求前建議依序檢查：

1. 產生新的 `requestId`
2. 若請求有 body，先取得最終要送出的原始字串
3. 依上方規則組出簽名載荷
4. 產生 `X-Api-Sign`
5. 送出請求後不要再改寫 body

## 不要猜測

- 不要自行把 HTTP method、path、query string、分隔符或換行加入簽名，除非公開契約明確要求
- 不要把空 body 替換成 `{}` 或其他佔位值
- 不要簽一個 JSON，卻送出另一個 JSON
- 不要記錄 `secretKey`
- 不要自行假設比目前文件更寬鬆的時間視窗

## 精確參考

實作或審查簽名程式碼時，請回到以下頁面核對：

- [`../../skill/references/shared/headers.zh-TW.md`](../../skill/references/shared/headers.zh-TW.md)
- [`../../skill/references/shared/signature-examples.zh-TW.md`](../../skill/references/shared/signature-examples.zh-TW.md)
- [`../../skill/domains/shared/auth-signing.zh-TW.md`](../../skill/domains/shared/auth-signing.zh-TW.md)
