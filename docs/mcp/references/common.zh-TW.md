# 公共參考

## 用途

本頁是第一階段所有 MCP 能力共享事實的公共入口。需要先確認公共執行期契約時，先看這裡，再進入具體能力頁。

## 公共執行期事實

目前每個接入方都應先確認這些共享事實：

- 必填請求標頭是 `X-Api-Key`、`X-Api-Timestamp`、`X-Api-RequestId`、`X-Api-Sign`
- 有請求體時，簽名載荷是 `timestamp + requestId + rawBody`
- 無請求體時，簽名載荷是 `timestamp + requestId`
- 簽名公式是 `Base64(HMAC_SHA256(UTF8(secretKey), UTF8(payload)))`
- 每次請求都應使用新的 `X-Api-RequestId`

## 公共回應事實

目前所有公開回應都使用同一個頂層包裝：

- `code`
- `msg`
- `data`

目前成功判斷規則：

- `code == "0"` 表示成功
- 任何其他 `code` 都表示失敗，或至少需要進一步處理

建議始終保存或記錄：

- `X-Api-RequestId`
- 返回的 `code`
- 返回的 `msg`
- 當涉及資金流或簽名排查時，保留原始請求與回應

## 公共錯誤事實

遇到問題時，建議優先檢查這些目前公開錯誤場景：

- `400`：參數無效、缺少請求標頭、時間戳格式不對或時間戳過期
- `401`：簽名不匹配
- `500`：服務端失敗
- `40009`：`merchantOrderId` 重複

已知目前公開錯誤文案之一：

- `Timestamp expired.`

## 能力參考入口

做目前端點與資料契約核對時，請回到這些精確 reference：

- headers：[`../../skill/references/shared/headers.zh-TW.md`](../../skill/references/shared/headers.zh-TW.md)
- 簽名示例：[`../../skill/references/shared/signature-examples.zh-TW.md`](../../skill/references/shared/signature-examples.zh-TW.md)
- 錯誤碼：[`../../skill/references/shared/error-codes.zh-TW.md`](../../skill/references/shared/error-codes.zh-TW.md)
- 端點：[`../../skill/references/crypto/endpoints.zh-TW.md`](../../skill/references/crypto/endpoints.zh-TW.md)
- 請求與回應模型：[`../../skill/references/crypto/request-response-models.zh-TW.md`](../../skill/references/crypto/request-response-models.zh-TW.md)
- 狀態：[`../../skill/references/crypto/statuses.zh-TW.md`](../../skill/references/crypto/statuses.zh-TW.md)
- 整合檢查清單：[`../../skill/references/crypto/integration-checklist.zh-TW.md`](../../skill/references/crypto/integration-checklist.zh-TW.md)

## 如何使用本頁

建議使用順序：

1. 先在本頁確認公共請求標頭與簽名規則
2. 再在本頁確認公共回應與錯誤語義
3. 再打開目標 MCP 能力頁
4. 最後回到上面連結的精確 reference，完成實作前與上線前核對

## 常見錯誤

- 把這個摘要頁當成產生最終 API 客戶端程式碼的唯一事實來源
- 忘記 payment query 的狀態目前是規範化字串，而 payout query 目前仍是原始 `int`
- 只處理業務 payload 欄位，忽略 `code` 和 `msg` 這類頂層包裝欄位
- 只保留商戶側訂單號，不保留 `X-Api-RequestId` 這類追蹤資訊
