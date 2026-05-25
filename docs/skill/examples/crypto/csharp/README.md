# C# Examples

## Purpose

These examples show minimal .NET request code for the current crypto create flows. They keep one `body` string for both HMAC signing and `HttpContent`, which is the key integration constraint.

## Runtime notes

- The samples assume .NET 6 or newer.
- Replace `baseUrl`, `apiKey`, and `secretKey` before use.
- Keep the serialized JSON body unchanged between signing and `POST`.

## Example files

- [Create payment](./create-payment.md)
- [Create payout](./create-payout.md)

## Related references

- [`../../../references/shared/headers.en.md`](../../../references/shared/headers.en.md)
- [`../../../references/shared/signature-examples.en.md`](../../../references/shared/signature-examples.en.md)
- [`../../../references/crypto/request-response-models.en.md`](../../../references/crypto/request-response-models.en.md)
