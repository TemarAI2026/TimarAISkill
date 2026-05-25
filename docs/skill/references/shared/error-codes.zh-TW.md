# 錯誤碼參考

## 常見錯誤碼

| 外部 code | 目前含義 | 常見觸發情境 | 下一步檢查 |
| --- | --- | --- | --- |
| `"0"` | 成功 | 請求處理成功。 | 繼續依正常業務流程處理回應資料。 |
| `400` | `ParameterInvalid` | 缺少請求標頭、時間戳格式無效、時間戳過期或參數驗證失敗。 | 先檢查必填欄位、四個請求標頭，以及是否出現 `Timestamp expired.`。 |
| `401` | `SignaturesNotMatch` | 簽名不匹配。 | 核對 `secretKey`、原始請求本文字串、`timestamp`、`requestId` 與簽名公式。 |
| `500` | `ServerError` | 伺服器異常，或沒有更具體的公開錯誤碼。 | 記錄 `X-Api-RequestId`，保留原始請求並視需要聯繫支援。 |
| `40001` | `CoinNotExist` | 幣種不存在或目前設定未啟用。 | 檢查 `currency` 是否符合已開通的業務設定。 |
| `40007` | `OrderNotFound` | 支付訂單不存在。 | 核對平台回傳的 `orderId` 是否正確，並確認訂單屬於目前商戶。 |
| `40008` | `WithdrawalNotFound` | 代付訂單不存在。 | 核對平台回傳的 `orderId` 是否正確，並確認查詢的是代付單。 |
| `40009` | `OutOrderNoIsExists` | 商戶訂單號已存在。 | 檢查 `merchantOrderId` 是否已提交過。 |
| `40012` | `WithdrawalHasBeenTurnedOff` | 代付功能目前已關閉。 | 先確認帳戶設定與代付開通狀態。 |
| `40021` | `BusinessFrozenOrInactive` | 商戶已凍結或未啟用。 | 檢查商戶狀態並聯繫支援。 |

## 說明

- 目前 Crypto V2 對外回應中的 `code` 是字串；成功固定回傳 `"0"`。
- 同一個錯誤碼可能對應不同公開訊息；`Timestamp expired.` 是目前已知的公開訊息之一。
- 排障時優先保留 `X-Api-RequestId`、原始請求本文與收到的 `code`/`msg`。
