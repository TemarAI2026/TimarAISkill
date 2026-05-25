# 錯誤與重試說明

## 用途

本頁彙總目前 MCP-facing 接入中的公共錯誤處理與重試邊界。

## 建議記錄什麼

應始終保留：

- `X-Api-RequestId`
- 請求路徑與方法
- `X-Api-Timestamp`
- 回應 `code`
- 回應 `msg`
- 原始請求體的安全副本或 body hash

不要記錄 `secretKey`。

## 優先排查順序

出錯後，建議至少先檢查：

1. 四個必填請求標頭是否齊全
2. 時間戳格式與時效性
3. 用於簽名的原始 body 是否與實際送出一致
4. API key 與 secret key 是否配對正確
5. 請求是否可能已被平台接受

## 目前常見公開錯誤場景

- `400`：參數無效、缺請求標頭、時間戳無效或已過期
- `401`：簽名不匹配
- `500`：服務端失敗
- `40007`：收單訂單不存在
- `40008`：代付訂單不存在
- `40009`：`merchantOrderId` 重複

已知目前公開錯誤文案之一：

- `Timestamp expired.`

## 重試規則

- 不要盲目重試 create-payment 或 create-payout
- 先用自己的訂單號與已保存的 `X-Api-RequestId` 做調查
- 對重複提交使用短期去重
- 只有在確認問題不是由簽名、請求標頭或重複業務識別導致時，才進入下一步重試判斷

## 常見錯誤

- 把 create 請求當成天然安全且天然冪等，直接重試
- 排查時不保留 `X-Api-RequestId`
- 記錄了敏感密鑰，卻沒記錄追蹤資訊
- 看到 `500` 就直接認定一定沒有產生下游副作用

## 精確來源參考

- [`../../skill/domains/shared/error-handling.zh-TW.md`](../../skill/domains/shared/error-handling.zh-TW.md)
- [`../../skill/references/shared/error-codes.zh-TW.md`](../../skill/references/shared/error-codes.zh-TW.md)
