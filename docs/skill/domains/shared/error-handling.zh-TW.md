# 錯誤處理

## 要記錄什麼

- `X-Api-RequestId`
- 請求 path 與 HTTP method
- `X-Api-Timestamp`
- 回應 `code` 與 `msg`
- 原始請求 body 的安全副本或 body hash

不要記錄 `secretKey`。除非你的安全政策允許，否則不要記錄完整簽名。

## 先檢查什麼

- 確認四個必填標頭都已帶上：`X-Api-Key`、`X-Api-Timestamp`、`X-Api-RequestId`、`X-Api-Sign`。
- 確認 timestamp 格式有效，且仍在允許的時間窗內。
- 確認你簽名的是實際送出的原始 body 字串。
- 確認 API key 有效，且與使用的 secret key 相互對應。

常見的標頭相關失敗包含：缺少標頭、timestamp 格式無效、timestamp 過期、key 無效、簽名不匹配。

## 重試指引

- 不要盲目重試建立類請求。
- 在假設是伺服器問題前，先重新檢查標頭、timestamp、request ID 與簽名。
- 如果請求可能已被接受，先用你自己的訂單參考與已保存的 `X-Api-RequestId` 排查。
- 對重複提交做短期去重，以降低重放風險。
