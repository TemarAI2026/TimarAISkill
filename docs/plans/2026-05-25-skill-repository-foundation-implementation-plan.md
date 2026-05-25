# Skill Repository Foundation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the repository-governance and maintenance documents that make `TimarAISkill` sustainable as the standalone public skill-documentation repository.

**Architecture:** Keep `docs/skill` as the published documentation tree and add a lightweight repository-maintenance layer around it. Use root-level governance files for repository policy, and `docs/` support files for structure mapping and upstream-sync workflow.

**Tech Stack:** Markdown, git, repository documentation conventions

---

## File Structure / Responsibility Map

- Create: `CONTRIBUTING.md` (`how contributors edit this repository safely`)
- Create: `CHANGELOG.md` (`notable documentation releases and structural changes`)
- Create: `docs/repository-map.md` (`where content belongs in this repository`)
- Create: `docs/maintenance-workflow.md` (`how to sync future business-repository changes into this repository`)
- Verify against: `README.md`
- Verify against: `docs/skill/README.md`
- Verify against: `docs/specs/2026-05-25-skill-repository-foundation-design.md`

### Task 1: Add contributor guidance

**Files:**
- Create: `CONTRIBUTING.md`

- [ ] **Step 1: Write the contributor guide**

```md
# Contributing

## Repository purpose

This repository stores public AI-facing integration assets and future skill-related documentation.

## What belongs here

- public routing, domain, reference, and example documents
- repository-maintenance docs that help contributors keep public docs consistent

## What does not belong here

- internal-only operational runbooks
- undocumented API behavior guesses
- single-language public changes without parity updates
```

- [ ] **Step 2: Add editing and verification rules**

```md
## Editing rules

- Keep public integration behavior in `docs/skill/`.
- Update `hub`, `domains`, `references`, and `examples` in the correct layer.
- Do not invent unsupported headers, fields, endpoints, webhook guarantees, or status meanings.

## Source verification

- Re-check controller actions, DTO models, and public API spec files before editing exact facts.
- Update references before examples if the contract changes.
```

- [ ] **Step 3: Add language-parity and review checklist**

```md
## Language parity

- Public document changes must be reflected in Simplified Chinese, English, and Traditional Chinese.
- Keep code blocks, identifiers, paths, and field names identical across languages.

## Pre-commit checklist

- Confirm source-of-truth files were reviewed.
- Confirm all affected languages were updated.
- Confirm placeholder and unfinished-content scans are clean.
- Confirm examples still match references.
```

- [ ] **Step 4: Verify the contributor guide exists and is readable**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\CONTRIBUTING.md'`
Expected: the file explains repository scope, editing rules, source verification, language parity, and a review checklist.

### Task 2: Add changelog structure

**Files:**
- Create: `CHANGELOG.md`

- [ ] **Step 1: Write the changelog header and format**

```md
# Changelog

All notable documentation and repository-structure changes in this repository should be recorded in this file.

The format is simple:

- date
- scope
- summary
```

- [ ] **Step 2: Add the initial repository entries**

```md
## 2026-05-25

- Scope: repository bootstrap
  Summary: imported the standalone `docs/skill` public integration asset set into `TimarAISkill`.

- Scope: repository foundation
  Summary: added contributor guidance, changelog structure, repository map, and maintenance workflow documents.
```

- [ ] **Step 3: Verify the changelog is present**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\CHANGELOG.md'`
Expected: the file includes a short format note and the two 2026-05-25 entries.

### Task 3: Add repository map

**Files:**
- Create: `docs/repository-map.md`

- [ ] **Step 1: Write the repository map overview**

```md
# Repository Map

## Purpose

This file explains where documentation belongs in `TimarAISkill` and how the repository is organized.
```

- [ ] **Step 2: Describe the root files and `docs/skill`**

```md
## Root files

- `README.md`: repository entrypoint
- `CONTRIBUTING.md`: contributor rules
- `CHANGELOG.md`: notable repository changes

## Public docs tree

- `docs/skill/`: public AI-facing integration assets
```

- [ ] **Step 3: Describe the `docs/skill` layers and future expansion**

```md
## `docs/skill` layers

- `hub/`: routing and domain map entry documents
- `domains/`: shared rules and domain task guides
- `references/`: exact contract facts
- `examples/`: runnable samples

## Expansion direction

- `crypto` is the current published domain
- `fiat` remains reserved for future release
- new domains should follow the same layered structure
```

- [ ] **Step 4: Verify the repository map**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\docs\repository-map.md'`
Expected: the file explains root responsibilities, `docs/skill`, layering, and future domain expansion.

### Task 4: Add maintenance workflow

**Files:**
- Create: `docs/maintenance-workflow.md`

- [ ] **Step 1: Write the maintenance workflow overview**

```md
# Maintenance Workflow

## Purpose

This workflow defines how to update `TimarAISkill` after an upstream API or behavior change in a business repository.
```

- [ ] **Step 2: Write the source-verification sequence**

```md
## Source verification first

1. Identify the upstream change.
2. Re-check the current controller actions.
3. Re-check the DTO or request-response models.
4. Re-check any public API spec documents that are already in use.
5. Compare the source files against the current public docs before editing.
```

- [ ] **Step 3: Write the document update order**

```md
## Update order

1. Update `references` when exact facts change.
2. Update `domains` when integration guidance changes.
3. Update `examples` when request or response usage changes.
4. Update root or repository-maintenance docs only if process or structure changed.
```

- [ ] **Step 4: Write parity and release checks**

```md
## Final checks

- Update Simplified Chinese, English, and Traditional Chinese together for public docs.
- Run placeholder and unfinished-content scans.
- Re-check examples against references.
- Confirm no unsupported behavior was added by assumption.
```

- [ ] **Step 5: Verify the maintenance workflow**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\docs\maintenance-workflow.md'`
Expected: the file documents upstream verification, update order, language parity, and release checks.

### Task 5: Final repository verification and commit

**Files:**
- Modify: repository git index only

- [ ] **Step 1: Run a repository structure check**

Run: `Get-ChildItem -Recurse -File 'E:\项目\TimarAISkill\TimarAISkill' | Select-Object FullName`
Expected: the repository contains `README.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `docs/repository-map.md`, `docs/maintenance-workflow.md`, `docs/specs/...`, and `docs/skill/...`.

- [ ] **Step 2: Run an unfinished-content scan**

Run: `rg -n "TODO|TBD|placeholder|fill in|later" 'E:\项目\TimarAISkill\TimarAISkill'`
Expected: no unfinished placeholders in the newly added governance and workflow docs.

- [ ] **Step 3: Stage the new files**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' add CONTRIBUTING.md CHANGELOG.md docs/repository-map.md docs/maintenance-workflow.md docs/plans/2026-05-25-skill-repository-foundation-implementation-plan.md
```

- [ ] **Step 4: Commit the repository foundation docs**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' commit -m "docs: add skill repository foundation docs"
```

## Self-Review

1. **Spec coverage:** The plan covers contributor guidance, changelog structure, repository map, maintenance workflow, and final verification.
2. **Placeholder scan:** The plan avoids TODO-style placeholders and includes explicit file paths and commands.
3. **Type consistency:** The same file paths and layering model are used consistently across all tasks.
