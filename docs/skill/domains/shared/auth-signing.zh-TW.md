# 驗證與簽名

## 必填標頭

- `X-Api-Key`：你的 API key。
- `X-Api-Timestamp`：請求時間，支援 Unix 毫秒或 ISO8601。
- `X-Api-RequestId`：用於追蹤與支援排查的唯一請求識別碼。
- `X-Api-Sign`：請求簽名。

每次請求都應使用新的 `X-Api-RequestId`。目前公開規則的時間窗大約是伺服器時間 `+/-30` 秒。

## 簽名原文

- 有 body 時：`timestamp + requestId + raw request body string`
- 無 body 時：`timestamp + requestId`

body 必須是你實際送出的原始字串。

## 簽名演算法

`X-Api-Sign = Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`

## 不要猜測

- 除非文件明確要求，否則不要自行加入 path、method、query string、分隔符或換行。
- 不要把空 body 換成 `{}` 或其他佔位內容。
- 不要在簽名後再格式化或重新序列化 JSON。
- 不要重用舊的 timestamp 或 request ID。

## 常見錯誤

- 簽名用的是一份 JSON，實際送出的卻是另一份。
- 使用超出允許時間窗的本地時間。
- 少帶四個必填標頭中的任何一個。
- API key 與 secret key 配對錯誤。
- 重用相同的 `X-Api-RequestId`，且未做短期去重，增加重放風險。
