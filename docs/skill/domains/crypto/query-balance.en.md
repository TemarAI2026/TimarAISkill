# Query Balance

## What this endpoint returns

Use `GET /api/v2/digital/balances` to retrieve the current account balance view by currency. The current public response includes these concepts:

- `currency`
- `availableBalance`
- `lockedBalance`
- `totalBalance`

`availableBalance` is the amount available for new operations, `lockedBalance` is the amount already reserved or frozen, and `totalBalance` is the sum of both.

## How to use it safely

Treat balance lookup as an account view, not an order result. It is useful for:

- Pre-checking funds before creating a payout
- Showing available and locked balances per currency in an admin or merchant UI
- Reconciliation or monitoring of balance movement over time

Do not rely on a single balance read as your only control for high-concurrency payout decisions. Your own business layer still needs to handle concurrent submissions, reservations, and rollback behavior.

## Read before implementation

Before implementation, confirm that:

- Your code handles balances per `currency`
- Your UI or service distinguishes `availableBalance` from `totalBalance`
- You have read the [crypto endpoints](../../references/crypto/endpoints.en.md)
- You have read the [request and response models](../../references/crypto/request-response-models.en.md)
