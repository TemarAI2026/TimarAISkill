# 請求與回應模型參考

## 頂層回應包裝

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `code` | `string` | 頂層業務碼；成功固定為 `"0"`。 |
| `msg` | `string` | 頂層提示或錯誤訊息。 |
| `data` | `object \| boolean \| array \| null` | 頂層業務資料，具體形狀依端點而定。 |

- 建立支付對外回傳 `CryptoApiResponse<CreateCryptoPaymentV2Response>`。
- 查詢支付對外回傳 `CryptoApiResponse<GetCryptoPaymentV2Response>`。
- 取消支付對外回傳 `CryptoApiResponse<bool>`。
- 建立代付對外回傳 `CryptoApiResponse<CreateCryptoPayoutV2Response>`。
- 查詢代付對外回傳 `CryptoApiResponse<GetCryptoPayoutV2Response>`。
- 查詢餘額對外回傳 `CryptoApiResponse<List<CryptoBalanceV2Response>>`。
- 下文各回應表描述的是頂層 `data` 欄位內部的目前結構。

## 建立支付請求

| 欄位 | 類型 | 必填 | 說明 |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | 是 | 商戶訂單號。 |
| `merchantUserId` | `string` | 是 | 商戶側使用者識別碼。 |
| `amount` | `decimal` | 是 | 訂單金額。 |
| `currency` | `string` | 是 | 幣種。 |
| `network` | `string` | 是 | 網路名稱。 |
| `returnUrl` | `string` | 否 | 支付完成後的跳轉網址。 |
| `cancelUrl` | `string` | 否 | 取消支付後的跳轉網址。 |

## 建立支付回應

目前對外線上的回應包裝為 `CryptoApiResponse<CreateCryptoPaymentV2Response>`，下表描述 `data` 內部欄位。

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `orderId` | `string` | 平台訂單號。 |
| `merchantOrderId` | `string` | 商戶訂單號。 |
| `paymentUrl` | `string` | 支付連結。 |
| `amount` | `decimal` | 訂單金額。 |
| `receiveAddress` | `string` | 收款地址。 |
| `currency` | `string` | 幣種。 |
| `network` | `string` | 網路名稱。 |
| `expiresInSeconds` | `int` | 支付有效期，單位為秒。 |

## 查詢支付回應

目前對外線上的回應包裝為 `CryptoApiResponse<GetCryptoPaymentV2Response>`，下表描述 `data` 內部欄位。

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `orderId` | `string` | 平台訂單號。 |
| `merchantOrderId` | `string` | 商戶訂單號。 |
| `merchantUserId` | `string` | 商戶側使用者識別碼。 |
| `amount` | `decimal` | 訂單金額。 |
| `status` | `string` | 目前對外線上的狀態字串，依現狀序列化為 `PENDING`、`SUCCESS`、`CANCEL`、`RISK` 等環境值；不是原始列舉物件。 |
| `currency` | `string` | 幣種。 |
| `network` | `string` | 網路名稱。 |
| `paidAmount` | `decimal` | 已支付金額。 |
| `paymentUrl` | `string` | 支付連結。 |
| `receiveAddress` | `string` | 收款地址。 |
| `expiresInSeconds` | `int` | 支付有效期，單位為秒。 |
| `fee` | `decimal` | 手續費。 |
| `feeCurrency` | `string` | 手續費幣種。 |
| `depositDetails` | `List<CryptoPaymentDepositDetailV2Response>` | 每項目前包含 `amount`、`txId`、`sourceAddress`。 |

## 建立代付請求

| 欄位 | 類型 | 必填 | 說明 |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | 是 | 商戶訂單號。 |
| `merchantUserId` | `string` | 是 | 商戶側使用者識別碼。 |
| `amount` | `decimal` | 是 | 訂單金額。 |
| `currency` | `string` | 是 | 幣種。 |
| `network` | `string` | 是 | 網路名稱。 |
| `withdrawAddress` | `string` | 是 | 提幣地址。 |

## 建立代付回應

目前對外線上的回應包裝為 `CryptoApiResponse<CreateCryptoPayoutV2Response>`，下表描述 `data` 內部欄位。

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `orderId` | `string` | 平台訂單號。 |
| `merchantOrderId` | `string` | 商戶訂單號。 |
| `merchantUserId` | `string` | 商戶側使用者識別碼。 |
| `amount` | `decimal` | 訂單金額。 |
| `fee` | `decimal` | 手續費。 |
| `currency` | `string` | 幣種。 |
| `network` | `string` | 網路名稱。 |
| `withdrawAddress` | `string` | 提幣地址。 |

## 查詢代付回應

目前對外線上的回應包裝為 `CryptoApiResponse<GetCryptoPayoutV2Response>`，下表描述 `data` 內部欄位。

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `orderId` | `string` | 平台訂單號。 |
| `merchantOrderId` | `string` | 商戶訂單號。 |
| `merchantUserId` | `string` | 商戶側使用者識別碼。 |
| `amount` | `decimal` | 訂單金額。 |
| `totalFee` | `decimal` | 總手續費。 |
| `networkFee` | `decimal` | 鏈上手續費。 |
| `serviceFee` | `decimal` | 服務費。 |
| `status` | `int` | 目前代付狀態值，見狀態參考。 |
| `currency` | `string` | 幣種。 |
| `network` | `string` | 網路名稱。 |
| `txId` | `string` | 鏈上交易號。 |
| `withdrawAddress` | `string` | 提幣地址。 |
| `sourceAddress` | `string` | 出款地址。 |
| `feeCurrency` | `string` | 手續費幣種。 |

## 查詢餘額回應

目前對外線上的回應包裝為 `CryptoApiResponse<List<CryptoBalanceV2Response>>`，下表描述 `data` 陣列中每個元素的欄位。

| 欄位 | 類型 | 說明 |
| --- | --- | --- |
| `currency` | `string` | 幣種。 |
| `availableBalance` | `decimal` | 可用餘額。 |
| `lockedBalance` | `decimal` | 凍結餘額。 |
| `totalBalance` | `decimal` | 總餘額。 |
