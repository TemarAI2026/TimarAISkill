# Contributing

## Repository purpose

`TemarAISkill` is the standalone repository for public AI-facing integration assets and future skill-related documentation. The main published content currently lives under `docs/skill/`.

## What belongs here

- public routing, domain, reference, and example documents
- repository-maintenance documents that help contributors keep public docs consistent
- support documentation that explains repository structure or update workflow

## What does not belong here

- internal-only operational runbooks
- undocumented API behavior guesses
- business logic source code copied from upstream repositories
- public document changes shipped in only one language

## Document layering rules

Use the existing `docs/skill` layers consistently:

- `hub/`: entry routing and document navigation
- `domains/`: integration guidance and business-flow instructions
- `references/`: exact headers, endpoints, models, statuses, and checklists
- `examples/`: runnable request examples and usage patterns

If an update changes exact contract facts, update `references` first. Only then update `domains` and `examples` if they are affected.

## Editing rules

- Keep public integration behavior in `docs/skill/`.
- Keep repository-governance and maintenance documents in `docs/` or the repository root.
- Do not invent unsupported headers, fields, endpoints, webhook guarantees, retry semantics, or status meanings.
- Use exact identifiers in code formatting.
- Keep examples aligned with the current public contract layer.

## Source verification expectations

Before editing public integration facts:

1. Re-check the current upstream controller actions.
2. Re-check the upstream DTO or request-response model definitions.
3. Re-check any current public API spec documents already used as references.
4. Compare the source files against the current public docs before editing.

If sources conflict, prefer the current controller and DTO behavior unless a newer published contract explicitly says otherwise.

## Language parity

Public document changes must preserve parity across:

- Simplified Chinese
- English
- Traditional Chinese

Rules:

- Keep code blocks, identifiers, paths, header names, field names, and endpoint paths identical across languages.
- Only explanatory prose should vary by language.
- Do not update one language and leave the other two behind for a separate pass.

## Suggested contribution flow

1. Identify the upstream change or documentation gap.
2. Verify the source-of-truth files.
3. Update `references` first if exact facts changed.
4. Update `domains` and `examples` as needed.
5. Apply the same logical change across all required languages.
6. Run repository checks before committing.

## Pre-commit checklist

- Confirm the relevant source-of-truth files were reviewed.
- Confirm all affected languages were updated.
- Confirm `references`, `domains`, and `examples` are still consistent with each other.
- Confirm no unsupported behavior was added by assumption.
- Confirm unfinished-content scans are clean.

## Commit guidance

- Use focused commits that group one logical documentation change.
- Prefer commit messages that describe the documentation outcome, such as `docs: update payout status mapping`.
