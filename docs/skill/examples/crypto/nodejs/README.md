# Node.js Examples

## Purpose

These examples show minimal Node.js request code for the current crypto create flows. They explicitly reuse the same `body` string for signing and for the outbound HTTP request.

## Runtime notes

- The samples assume Node.js 18 or newer with built-in `fetch`.
- Replace `baseUrl`, `apiKey`, and `secretKey` before use.
- Keep `JSON.stringify` output in one variable and do not regenerate it after signing.

## Example files

- [Create payment](./create-payment.md)
- [Create payout](./create-payout.md)

## Related references

- [`../../../references/shared/headers.en.md`](../../../references/shared/headers.en.md)
- [`../../../references/shared/signature-examples.en.md`](../../../references/shared/signature-examples.en.md)
- [`../../../references/crypto/request-response-models.en.md`](../../../references/crypto/request-response-models.en.md)
