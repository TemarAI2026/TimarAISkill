# MCP Public Documentation Skeleton Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the first-phase `docs/mcp` public documentation skeleton and expose it as the MCP-first public entrypoint in `TimarAISkill`.

**Architecture:** Create a new `docs/mcp/` tree for MCP overview and capability routing while keeping `docs/skill/` in place as the AI-facing guidance layer. The new MCP docs stay high-level and route readers into the current published contract details under `docs/skill`.

**Tech Stack:** Markdown, repository documentation, existing `docs/skill` references

---

## File Structure / Responsibility Map

- Create: `docs/mcp/README.md`
- Create: `docs/mcp/README.en.md`
- Create: `docs/mcp/README.zh-TW.md`
- Create: `docs/mcp/overview/architecture.md`
- Create: `docs/mcp/overview/architecture.en.md`
- Create: `docs/mcp/overview/architecture.zh-TW.md`
- Create: `docs/mcp/overview/environments.md`
- Create: `docs/mcp/overview/environments.en.md`
- Create: `docs/mcp/overview/environments.zh-TW.md`
- Create: `docs/mcp/overview/auth-signing.md`
- Create: `docs/mcp/overview/auth-signing.en.md`
- Create: `docs/mcp/overview/auth-signing.zh-TW.md`
- Create: `docs/mcp/capabilities/payment.md`
- Create: `docs/mcp/capabilities/payment.en.md`
- Create: `docs/mcp/capabilities/payment.zh-TW.md`
- Create: `docs/mcp/capabilities/payout.md`
- Create: `docs/mcp/capabilities/payout.en.md`
- Create: `docs/mcp/capabilities/payout.zh-TW.md`
- Create: `docs/mcp/capabilities/balance.md`
- Create: `docs/mcp/capabilities/balance.en.md`
- Create: `docs/mcp/capabilities/balance.zh-TW.md`
- Create: `docs/mcp/capabilities/notifications.md`
- Create: `docs/mcp/capabilities/notifications.en.md`
- Create: `docs/mcp/capabilities/notifications.zh-TW.md`
- Create: `docs/mcp/references/common.md`
- Create: `docs/mcp/references/common.en.md`
- Create: `docs/mcp/references/common.zh-TW.md`
- Modify: `README.md`
- Modify: `docs/repository-map.md`

### Task 1: Create MCP public entry documents

**Files:**
- Create: `docs/mcp/README.md`
- Create: `docs/mcp/README.en.md`
- Create: `docs/mcp/README.zh-TW.md`

- [ ] **Step 1: Write the Simplified Chinese MCP entrypoint**

The file should explain:

- what MCP is
- who it is for
- that merchant onboarding is a prerequisite
- the reading order into overview and capability pages

- [ ] **Step 2: Mirror the English and Traditional Chinese entrypoints**

Use the same structure and topic order for:

- `docs/mcp/README.en.md`
- `docs/mcp/README.zh-TW.md`

- [ ] **Step 3: Verify README section parity**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp\README*.md' -Pattern '^# |^## '`
Expected: the three MCP README files use the same section structure.

### Task 2: Create MCP overview documents

**Files:**
- Create: `docs/mcp/overview/architecture.md`
- Create: `docs/mcp/overview/architecture.en.md`
- Create: `docs/mcp/overview/architecture.zh-TW.md`
- Create: `docs/mcp/overview/environments.md`
- Create: `docs/mcp/overview/environments.en.md`
- Create: `docs/mcp/overview/environments.zh-TW.md`
- Create: `docs/mcp/overview/auth-signing.md`
- Create: `docs/mcp/overview/auth-signing.en.md`
- Create: `docs/mcp/overview/auth-signing.zh-TW.md`

- [ ] **Step 1: Write `architecture*`**

The files should explain:

- MCP as the capability base
- `skill`, `x402`, and future protocols as upper-layer adapters
- what MCP owns and what remains outside first-phase scope

- [ ] **Step 2: Write `environments*`**

The files should explain:

- sandbox vs production selection
- runtime prerequisites
- that merchant registration and enablement are preconditions, not per-request runtime steps

- [ ] **Step 3: Write `auth-signing*`**

The files should explain:

- `apiKey`, `secretKey`, timestamp, request ID, and signature usage
- where to look for exact header names and payload rules in `docs/skill/references`

- [ ] **Step 4: Verify overview structure**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp\overview\*.md' -Pattern '^# |^## '`
Expected: each topic has three aligned language files.

### Task 3: Create MCP capability documents

**Files:**
- Create: `docs/mcp/capabilities/payment.md`
- Create: `docs/mcp/capabilities/payment.en.md`
- Create: `docs/mcp/capabilities/payment.zh-TW.md`
- Create: `docs/mcp/capabilities/payout.md`
- Create: `docs/mcp/capabilities/payout.en.md`
- Create: `docs/mcp/capabilities/payout.zh-TW.md`
- Create: `docs/mcp/capabilities/balance.md`
- Create: `docs/mcp/capabilities/balance.en.md`
- Create: `docs/mcp/capabilities/balance.zh-TW.md`
- Create: `docs/mcp/capabilities/notifications.md`
- Create: `docs/mcp/capabilities/notifications.en.md`
- Create: `docs/mcp/capabilities/notifications.zh-TW.md`

- [ ] **Step 1: Write `payment*`**

The files should explain the MCP payment capability set at a high level and route readers into the current published create/query/cancel payment docs.

- [ ] **Step 2: Write `payout*`**

The files should explain the MCP payout capability set at a high level and route readers into the current published payout docs.

- [ ] **Step 3: Write `balance*`**

The files should explain the balance capability at a high level and route readers into the current published balance docs.

- [ ] **Step 4: Write `notifications*`**

The files should explain callback and asynchronous notification expectations and route readers into current webhook guidance.

- [ ] **Step 5: Verify capability structure**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp\capabilities\*.md' -Pattern '^# |^## '`
Expected: each capability topic has three aligned language files.

### Task 4: Create MCP common reference entry documents

**Files:**
- Create: `docs/mcp/references/common.md`
- Create: `docs/mcp/references/common.en.md`
- Create: `docs/mcp/references/common.zh-TW.md`

- [ ] **Step 1: Write the common reference entry files**

The files should summarize:

- shared headers
- signature rules
- error handling references
- status references
- the fact that exact contract details remain in `docs/skill/references` for the current published capability set

- [ ] **Step 2: Verify reference structure**

Run: `Select-String -Path 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp\references\*.md' -Pattern '^# |^## '`
Expected: the common reference files align across languages.

### Task 5: Expose MCP in repository entrypoints

**Files:**
- Modify: `README.md`
- Modify: `docs/repository-map.md`

- [ ] **Step 1: Update `README.md`**

Add MCP as the primary public entrypoint by linking:

- `docs/mcp/README.md`
- `docs/mcp/README.en.md`
- `docs/mcp/README.zh-TW.md`

while keeping `docs/skill` visible as the AI-facing guidance layer.

- [ ] **Step 2: Update `docs/repository-map.md`**

Add `docs/mcp/` as:

- the MCP-first public documentation home
- separate from `docs/skill/`, which remains the AI-facing guidance layer

- [ ] **Step 3: Verify entrypoint wording**

Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\README.md'`
Run: `Get-Content -Raw 'E:\项目\TimarAISkill\TimarAISkill\docs\repository-map.md'`
Expected: both files expose `docs/mcp/` clearly without conflicting with the current `docs/skill/` role.

### Task 6: Final verification and commit

**Files:**
- Modify: repository git index only

- [ ] **Step 1: Run unfinished-content scans on new MCP docs**

Run: `rg -n "TODO|TBD|placeholder|later|fill in" 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp'`
Expected: no unfinished-content markers in the new MCP docs.

- [ ] **Step 2: Run a repository structure check**

Run: `Get-ChildItem -Recurse -File 'E:\项目\TimarAISkill\TimarAISkill\docs\mcp' | Select-Object FullName`
Expected: the MCP README, overview, capabilities, and references files all exist.

- [ ] **Step 3: Stage the MCP documentation changes**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' add README.md docs/repository-map.md docs/mcp docs/plans/2026-05-25-mcp-public-documentation-skeleton-implementation-plan.md
```

- [ ] **Step 4: Commit the MCP documentation skeleton**

```bash
git -C 'E:\项目\TimarAISkill\TimarAISkill' commit -m "docs: add mcp public documentation skeleton"
```

## Self-Review

1. **Spec coverage:** The plan covers MCP entry docs, overview docs, capability docs, common reference docs, and repository entrypoint updates.
2. **Placeholder scan:** The plan uses exact file paths and explicit verification commands.
3. **Type consistency:** The plan consistently treats `docs/mcp/` as the MCP-first public home and `docs/skill/` as the AI-facing guidance layer.
