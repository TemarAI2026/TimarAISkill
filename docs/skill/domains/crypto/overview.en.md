# Crypto Overview

## What this domain covers

The crypto domain covers the current public digital-asset integration surface. It helps external integrators and AI coding assistants understand the live V2 endpoints, the active field set, and the main differences between payment and payout flows.

The current public endpoints are:

- `POST /api/v2/digital/payments`
- `GET /api/v2/digital/payments/{orderId}`
- `POST /api/v2/digital/payments/{orderId}/cancel`
- `POST /api/v2/digital/payouts`
- `GET /api/v2/digital/payouts/{orderId}`
- `GET /api/v2/digital/balances`

## Supported tasks

- Create a crypto payment and receive `paymentUrl`, `receiveAddress`, and `expiresInSeconds`
- Query payment status and payment details
- Cancel a payment that has not completed yet
- Create a crypto payout and track the withdrawal result
- Query balances with `availableBalance`, `lockedBalance`, and `totalBalance`
- Plan webhook handling, while confirming the latest public docs and your integration contract before implementation

## Payment vs payout

Payments are incoming-funds flows. They start with `POST /api/v2/digital/payments` and usually drive the user to a `paymentUrl` or `receiveAddress` so the payment can be completed before expiry.

Payouts are outgoing-funds flows. They start with `POST /api/v2/digital/payouts`, require a `withdrawAddress`, and put more emphasis on address accuracy, balance availability, and post-submission status tracking.

Do not mix the two flows or their fields. For example, `returnUrl` and `cancelUrl` belong to the current payment-create model, while `withdrawAddress` belongs to the current payout-create model.

## Read next

- [Create payment](./create-payment.en.md)
- [Create payout](./create-payout.en.md)
- [Query order](./query-order.en.md)
- [Query balance](./query-balance.en.md)
- [Handle webhook](./handle-webhook.en.md)
- [Crypto endpoints](../../references/crypto/endpoints.en.md)
- [Request and response models](../../references/crypto/request-response-models.en.md)
