# MCP-First Documentation Reframing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reframe the repository and top-level public docs so `TimarAISkill` officially presents MCP as the capability base and `skill / x402 / future protocols` as upper-layer integration adapters.

**Architecture:** Keep the current `docs/skill` tree intact for now, but update the repository entrypoints and MCP-facing guidance so the documentation clearly explains the new layering model. This is a documentation-framing change, not a full content-structure rewrite.

**Tech Stack:** Markdown, repository documentation, existing `docs/skill` content

---

## File Structure / Responsibility Map

- Modify: `README.md`
- Modify: `docs/repository-map.md`
- Modify: `docs/skill/README.md`
- Modify: `docs/skill/README.en.md`
- Modify: `docs/skill/README.zh-TW.md`
- Modify: `docs/skill/hub/integration-router.md`
- Modify: `docs/skill/hub/integration-router.en.md`
- Modify: `docs/skill/hub/integration-router.zh-TW.md`
- Modify: `docs/skill/hub/domain-map.md`
- Modify: `docs/skill/hub/domain-map.en.md`
- Modify: `docs/skill/hub/domain-map.zh-TW.md`
- Verify against: `docs/specs/2026-05-25-mcp-first-open-integration-architecture-design.md`

### Task 1: Update repository root positioning

**Files:**
- Modify: `README.md`
- Modify: `docs/repository-map.md`

- [ ] **Step 1: Update `README.md` to introduce MCP-first architecture**

Add or revise content so the root README clearly says:

- this repository is for public AI-facing integration assets
- MCP is the primary open capability base
- `skill`, `x402`, and future protocols are adapter or trigger layers above MCP
- the current published domain content remains crypto-first

- [ ] **Step 2: Update `docs/repository-map.md` to reflect MCP-first framing**

Add or revise content so the repository map explains:

- `docs/skill/` is the public guidance layer for using MCP-backed capabilities
- the current folders remain valid, but they should be interpreted through MCP-first architecture
- future docs may add MCP overview and capability-domain material without breaking the layered structure

- [ ] **Step 3: Verify root positioning docs**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\README.md'`
Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\docs\repository-map.md'`
Expected: both files consistently describe MCP as the capability base and `skill / x402` as upper-layer adapters.

### Task 2: Reframe the public `docs/skill` entrypoints

**Files:**
- Modify: `docs/skill/README.md`
- Modify: `docs/skill/README.en.md`
- Modify: `docs/skill/README.zh-TW.md`

- [ ] **Step 1: Update the Simplified Chinese entrypoint**

Revise the README so it clearly explains:

- this directory helps AI assistants and external integrators understand MCP-backed open capabilities
- `skill` is the AI-facing integration guidance and trigger layer
- current content still focuses on the published crypto domain
- future protocols should route into MCP rather than define separate business cores

- [ ] **Step 2: Update the English and Traditional Chinese entrypoints**

Mirror the same structure and meaning in:

- `docs/skill/README.en.md`
- `docs/skill/README.zh-TW.md`

- [ ] **Step 3: Verify section parity**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\README*.md' -Pattern '^# |^## '`
Expected: the three README files keep the same section structure while reflecting the MCP-first framing.

### Task 3: Reframe hub routing documents

**Files:**
- Modify: `docs/skill/hub/integration-router.md`
- Modify: `docs/skill/hub/integration-router.en.md`
- Modify: `docs/skill/hub/integration-router.zh-TW.md`
- Modify: `docs/skill/hub/domain-map.md`
- Modify: `docs/skill/hub/domain-map.en.md`
- Modify: `docs/skill/hub/domain-map.zh-TW.md`

- [ ] **Step 1: Update `integration-router*` files**

Revise the router docs so they explain:

- the reader is choosing an MCP capability path
- `skill` and other protocol surfaces are entry adapters
- the linked docs describe MCP-backed capability usage for the current published domain

- [ ] **Step 2: Update `domain-map*` files**

Revise the domain map docs so they explain:

- the public docs still use the current layered structure
- MCP is the capability base beneath the current public guidance
- `crypto` is the currently published domain
- `fiat` remains reserved

- [ ] **Step 3: Verify hub parity**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\*.md' -Pattern '^# |^## '`
Expected: the hub files remain structurally aligned while adopting MCP-first language.

### Task 4: Final consistency check and commit

**Files:**
- Modify: repository git index only

- [ ] **Step 1: Run unfinished-content scans on changed public entry docs**

Run: `rg -n "TODO|TBD|placeholder|later|fill in" 'E:\项目\TimarAISkill\TimarAISkill\README.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\repository-map.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\README.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\README.en.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\README.zh-TW.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\integration-router.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\integration-router.en.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\integration-router.zh-TW.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\domain-map.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\domain-map.en.md' 'E:\项目\TimarAISkill\TimarAISkill\docs\skill\hub\domain-map.zh-TW.md'`
Expected: no unfinished-content markers in the reframed entry documents.

- [ ] **Step 2: Stage the plan and documentation changes**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' add README.md docs/repository-map.md docs/skill/README.md docs/skill/README.en.md docs/skill/README.zh-TW.md docs/skill/hub/integration-router.md docs/skill/hub/integration-router.en.md docs/skill/hub/integration-router.zh-TW.md docs/skill/hub/domain-map.md docs/skill/hub/domain-map.en.md docs/skill/hub/domain-map.zh-TW.md docs/plans/2026-05-25-mcp-first-documentation-reframing-implementation-plan.md
```

- [ ] **Step 3: Commit the reframing change**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' commit -m "docs: reframe public docs around mcp-first architecture"
```

## Self-Review

1. **Spec coverage:** The plan updates the repository root, repository map, public skill entrypoints, and hub routing docs to match the MCP-first architecture.
2. **Placeholder scan:** The plan includes exact file paths and explicit verification commands.
3. **Type consistency:** The plan consistently uses MCP as the capability base and `skill / x402 / future protocols` as upper-layer adapters.
