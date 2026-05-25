# Authentication and Signing

## Runtime credentials

In phase one, MCP runtime usage centers on these inputs:

- `apiKey`
- `secretKey`
- `timestamp`
- `requestId`
- `sign`

## What to read next

This page only gives the MCP-level summary.  
For exact contract facts, continue to the currently published reference material:

- [`../../skill/references/shared/headers.en.md`](../../skill/references/shared/headers.en.md)
- [`../../skill/references/shared/signature-examples.en.md`](../../skill/references/shared/signature-examples.en.md)
- [`../../skill/domains/shared/auth-signing.en.md`](../../skill/domains/shared/auth-signing.en.md)

## Runtime rule

In the current published capability set:

- generate a fresh `requestId` for every request
- sign the real raw content that will actually be sent
- do not rebuild or reformat the request body after signing
