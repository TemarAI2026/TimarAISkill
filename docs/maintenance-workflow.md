# Maintenance Workflow

## Purpose

This workflow defines how to update `TemarAISkill` after an upstream API or behavior change in a business repository.

## When to use this workflow

Use this workflow when:

- controller routes changed
- request or response fields changed
- signing rules changed
- public status semantics changed
- example requests no longer match the published contract

## Source verification first

Before editing public documentation:

1. Identify the upstream change.
2. Re-check the current controller actions.
3. Re-check the DTO or request-response model definitions.
4. Re-check the public API spec documents already used by the repository.
5. Compare the source files against the current public docs before editing anything.

Do not start from the examples layer and work backward. Exact facts must be verified first.

## Update order

Use this order whenever contract behavior changes:

1. Update `references` when exact facts change.
2. Update `domains` when integration guidance changes.
3. Update `examples` when request or response usage changes.
4. Update repository-governance docs only if structure or process changed.

This keeps guides and examples anchored to the current fact layer.

## Language update rules

For public documentation:

- update Simplified Chinese, English, and Traditional Chinese together
- keep code blocks, identifiers, paths, headers, fields, and endpoints identical across languages
- change only explanatory prose per language

Do not leave a public change half-translated.

## Review questions before commit

Ask these questions before publishing:

- Did the upstream controller and DTO files actually support this change?
- Did `references` move before `domains` and `examples`?
- Do examples still match the latest headers, paths, and body fields?
- Did all three public language variants receive the same logical update?
- Did the change introduce any unsupported assumptions?

## Final checks

Run these checks before commit when applicable:

- unfinished-content scan
- cross-language parity review
- references-versus-examples consistency review
- repository structure check if new files were added

## Escalation rule

If current public docs and upstream source files disagree in a way that changes external behavior, stop and resolve the source-of-truth decision before publishing. Do not smooth over the mismatch with vague language.
