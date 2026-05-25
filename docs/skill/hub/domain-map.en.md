# Domain Map

## Current document layers

The public integration documents are organized into a small set of layers so readers can move from orientation to implementation detail:

- Hub documents such as [Integration Router](./integration-router.en.md) help you choose a reading path.
- Shared domain guides such as [Auth Signing](../domains/shared/auth-signing.en.md), [Error Handling](../domains/shared/error-handling.en.md), and [Response Conventions](../domains/shared/response-conventions.en.md) describe rules reused across domains.
- Domain guides such as [Crypto Overview](../domains/crypto/overview.en.md) explain business flows for a specific domain.
- Reference documents such as [crypto endpoints](../references/crypto/endpoints.en.md), [request and response models](../references/crypto/request-response-models.en.md), and [shared headers](../references/shared/headers.en.md) provide implementation details.
- Examples such as [Node.js crypto example](../examples/crypto/nodejs/README.md), [C# crypto example](../examples/crypto/csharp/README.md), and [cURL crypto example](../examples/crypto/curl/README.md) show end-to-end usage patterns.

## Available domains

- Shared: Cross-domain rules that apply before or alongside any business flow, including signing, response conventions, and error handling.
- Crypto: The current public domain. This domain includes payment, payout, order query, webhook, and balance capabilities for the published digital asset endpoints.
- Fiat: A reserved future domain. Its entry point already exists at [Fiat README](../domains/fiat/README.en.md), but no public fiat flow, field set, or status mapping is published yet.

## Expansion note

As new domains are published, the shared layer remains the common foundation and each domain adds its own guides, references, and examples. External integrators and AI coding assistants should start at the hub, confirm the domain, then follow the linked shared and domain-specific documents before generating code.
