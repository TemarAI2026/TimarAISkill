# Request and Response Models Reference

## Top-Level Response Wrapper

| Field | Type | Description |
| --- | --- | --- |
| `code` | `string` | Top-level business code. Success is always `"0"`. |
| `msg` | `string` | Top-level message or error text. |
| `data` | `object \| boolean \| array \| null` | Top-level business payload whose shape depends on the endpoint. |

- Create payment returns `CryptoApiResponse<CreateCryptoPaymentV2Response>`.
- Query payment returns `CryptoApiResponse<GetCryptoPaymentV2Response>`.
- Cancel payment returns `CryptoApiResponse<bool>`.
- Create payout returns `CryptoApiResponse<CreateCryptoPayoutV2Response>`.
- Query payout returns `CryptoApiResponse<GetCryptoPayoutV2Response>`.
- Query balance returns `CryptoApiResponse<List<CryptoBalanceV2Response>>`.
- The response tables below describe the current shape inside the top-level `data` field.

## Create Payment Request

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | Yes | Merchant order number. |
| `merchantUserId` | `string` | Yes | Merchant-side user identifier. |
| `amount` | `decimal` | Yes | Order amount. |
| `currency` | `string` | Yes | Currency. |
| `network` | `string` | Yes | Network name. |
| `returnUrl` | `string` | No | Redirect URL after payment completion. |
| `cancelUrl` | `string` | No | Redirect URL after payment cancellation. |

## Create Payment Response

The public wire response is `CryptoApiResponse<CreateCryptoPaymentV2Response>`. The table below describes fields inside `data`.

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | `string` | Platform order ID. |
| `merchantOrderId` | `string` | Merchant order number. |
| `paymentUrl` | `string` | Payment URL. |
| `amount` | `decimal` | Order amount. |
| `receiveAddress` | `string` | Deposit address. |
| `currency` | `string` | Currency. |
| `network` | `string` | Network name. |
| `expiresInSeconds` | `int` | Payment validity period in seconds. |

## Query Payment Response

The public wire response is `CryptoApiResponse<GetCryptoPaymentV2Response>`. The table below describes fields inside `data`.

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | `string` | Platform order ID. |
| `merchantOrderId` | `string` | Merchant order number. |
| `merchantUserId` | `string` | Merchant-side user identifier. |
| `amount` | `decimal` | Order amount. |
| `status` | `string` | Current public wire status string, currently serialized as ambient values such as `PENDING`, `SUCCESS`, `CANCEL`, and `RISK`, not as a raw enum object. |
| `currency` | `string` | Currency. |
| `network` | `string` | Network name. |
| `paidAmount` | `decimal` | Amount already paid. |
| `paymentUrl` | `string` | Payment URL. |
| `receiveAddress` | `string` | Deposit address. |
| `expiresInSeconds` | `int` | Payment validity period in seconds. |
| `fee` | `decimal` | Fee amount. |
| `feeCurrency` | `string` | Fee currency. |
| `depositDetails` | `List<CryptoPaymentDepositDetailV2Response>` | Each item currently includes `amount`, `txId`, and `sourceAddress`. |

## Create Payout Request

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `merchantOrderId` | `string` | Yes | Merchant order number. |
| `merchantUserId` | `string` | Yes | Merchant-side user identifier. |
| `amount` | `decimal` | Yes | Order amount. |
| `currency` | `string` | Yes | Currency. |
| `network` | `string` | Yes | Network name. |
| `withdrawAddress` | `string` | Yes | Withdrawal address. |

## Create Payout Response

The public wire response is `CryptoApiResponse<CreateCryptoPayoutV2Response>`. The table below describes fields inside `data`.

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | `string` | Platform order ID. |
| `merchantOrderId` | `string` | Merchant order number. |
| `merchantUserId` | `string` | Merchant-side user identifier. |
| `amount` | `decimal` | Order amount. |
| `fee` | `decimal` | Fee amount. |
| `currency` | `string` | Currency. |
| `network` | `string` | Network name. |
| `withdrawAddress` | `string` | Withdrawal address. |

## Query Payout Response

The public wire response is `CryptoApiResponse<GetCryptoPayoutV2Response>`. The table below describes fields inside `data`.

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | `string` | Platform order ID. |
| `merchantOrderId` | `string` | Merchant order number. |
| `merchantUserId` | `string` | Merchant-side user identifier. |
| `amount` | `decimal` | Order amount. |
| `totalFee` | `decimal` | Total fee. |
| `networkFee` | `decimal` | On-chain network fee. |
| `serviceFee` | `decimal` | Service fee. |
| `status` | `int` | Current payout status value. See the statuses reference. |
| `currency` | `string` | Currency. |
| `network` | `string` | Network name. |
| `txId` | `string` | On-chain transaction ID. |
| `withdrawAddress` | `string` | Withdrawal address. |
| `sourceAddress` | `string` | Source address. |
| `feeCurrency` | `string` | Fee currency. |

## Query Balance Response

The public wire response is `CryptoApiResponse<List<CryptoBalanceV2Response>>`. The table below describes each element inside the `data` array.

| Field | Type | Description |
| --- | --- | --- |
| `currency` | `string` | Currency. |
| `availableBalance` | `decimal` | Available balance. |
| `lockedBalance` | `decimal` | Locked balance. |
| `totalBalance` | `decimal` | Total balance. |
