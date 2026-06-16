# Skill Repository Foundation Design

## Summary

This design defines the next foundation step for the standalone `TemarAISkill` repository. The repository already contains the first imported public AI-facing integration asset set under `docs/skill`. The next step is to make the repository maintainable as a dedicated long-term home for public skill-related documentation.

The focus of this phase is not to expand business scope. The focus is to add repository-level guidance and maintenance workflow documents so future contributors can keep the documentation set consistent, source-backed, and usable for external integrators and AI coding assistants.

## Goals

- Establish `TemarAISkill` as the dedicated repository for public skill and AI-facing integration documentation.
- Keep `docs/skill` as the primary published document tree.
- Add repository-level guidance so future updates follow the same structure and quality rules.
- Define a repeatable maintenance workflow for syncing business-repository API changes into this repository.
- Protect multilingual parity across Simplified Chinese, English, and Traditional Chinese.

## Non-Goals

- This phase does not redesign the existing `docs/skill` information architecture.
- This phase does not add new payment domains beyond the already reserved fiat entry.
- This phase does not automate synchronization from business repositories.
- This phase does not introduce internal-only documentation that should not be public.

## Current Context

The repository currently contains:

- a root `README.md`
- the imported `docs/skill` tree
- multilingual public documentation for the crypto domain

The repository does not yet contain:

- a contributor guide
- a changelog
- a repository map
- a documented maintenance workflow

Without these files, future editors can still update content, but they lack a clear contract for where changes belong, how to verify them, and how to keep cross-language consistency.

## Recommended Approach

Use a lightweight repository-governance layer around the existing `docs/skill` tree.

This approach adds only a few files while solving the highest-value maintenance gaps:

- `CONTRIBUTING.md`
- `CHANGELOG.md`
- `docs/repository-map.md`
- `docs/maintenance-workflow.md`

### Why this approach

- It preserves the current document layout without churn.
- It keeps the repository easy to understand for external readers.
- It gives maintainers clear rules before the repository grows larger.
- It avoids premature tooling or automation.

## Alternatives Considered

### Option 1: Keep only `docs/skill`

Pros:

- Minimal repository footprint
- No extra maintenance docs to write now

Cons:

- Future editors must infer structure and update rules
- Language drift risk remains high
- No stable sync process from source-of-truth repositories

### Option 2: Add full repository governance now

Pros:

- Very explicit process and repository management
- Easier onboarding for future maintainers

Cons:

- Too heavy for the current repository size
- Adds process overhead before it is needed

### Option 3: Add a minimal foundation layer now

Pros:

- Best balance of clarity and cost
- Supports immediate maintenance needs
- Leaves room for future automation or governance

Cons:

- Some additional policies may still need to be added in future revisions

Recommendation: Option 3.

## Files to Add

### `CONTRIBUTING.md`

Purpose:

- explain how to contribute changes to this repository
- define the document layering model
- explain source-of-truth expectations
- define multilingual update rules

Must include:

- contribution scope
- editing rules
- source verification expectations
- language parity expectations
- review checklist before commit

### `CHANGELOG.md`

Purpose:

- record notable repository-level documentation releases and structural changes

Must include:

- initial import entry for the standalone repository
- a simple changelog format maintainers can continue using

### `docs/repository-map.md`

Purpose:

- explain what each top-level area of the repository is for
- help maintainers quickly route future additions to the right location

Must include:

- root file descriptions
- `docs/skill` purpose
- meaning of `hub`, `domains`, `references`, and `examples`
- intended future expansion direction

### `docs/maintenance-workflow.md`

Purpose:

- define how maintainers update this repository after API or controller changes in the business repository

Must include:

- identify source-of-truth files first
- compare controller, DTO, and current public docs
- update references before examples if facts change
- update all three languages together
- run parity and unfinished-content checks before publishing

## Structure Rules

The repository should follow these rules after this phase:

- `docs/skill` remains the public product-facing entrypoint.
- Root files explain repository purpose and contribution rules.
- `docs/` may contain repository-maintenance documents that support `docs/skill`.
- Public integration behavior should remain in `docs/skill`, not be split across unrelated root files.

## Maintenance Workflow Design

Future updates should follow this order:

1. Detect the upstream API or behavior change.
2. Re-check source-of-truth files such as controller actions, DTO models, and public API spec documents.
3. Update exact fact documents first, especially `references`.
4. Update domain guides if the integration guidance changes.
5. Update examples if request shape, response shape, headers, or signature behavior changed.
6. Apply the same logical change to Simplified Chinese, English, and Traditional Chinese.
7. Run consistency checks before committing.

This order is intentional. Exact facts should move first so guides and examples do not drift from the contract layer.

## Acceptance Criteria

This phase is complete when:

1. The repository contains contributor guidance, changelog, repository map, and maintenance workflow documents.
2. A future maintainer can understand where to place new public docs without reading repository history.
3. The maintenance workflow clearly says to verify controller and DTO behavior before changing public docs.
4. The workflow clearly requires three-language parity for public document changes.
5. The added files support the current `docs/skill` structure without forcing a reorganization.

## Risks and Mitigations

### Risk: contributors update only one language

Mitigation:

- document language parity as an explicit contribution rule
- include it in the contributor checklist

### Risk: future updates change examples but not references

Mitigation:

- document update ordering in the maintenance workflow
- require fact-layer verification first

### Risk: this repository accumulates mixed internal and external docs

Mitigation:

- document that `docs/skill` remains the public-facing area
- keep repository-maintenance docs separate in `docs/`

## Implementation Scope

This phase should only create the four governance and workflow files listed above and update the repository root documentation only if needed for consistency.

## Next Step

After this design is approved and written, implement the four repository-foundation documents, verify the repository structure, and commit the changes to the current feature branch.
