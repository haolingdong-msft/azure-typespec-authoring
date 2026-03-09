---
name: azure-typespec-author
description: "Author or modify Azure TypeSpec API specifications in the azure-rest-api-specs repository. USE FOR: Any task that creates, modifies, or troubleshoots .tsp files or TypeSpec API specifications — including but not limited to API versioning (e.g. add new preview version, add new stable version), ARM or data-plane resource definitions (tracked, proxy, extension, child resources), resource operations (CRUD, PATCH, custom actions, async/LRO), models, enums, unions, properties, decorators, constraints, and swagger-to-TypeSpec conversion. DO NOT USE FOR: SDK generation from TypeSpec, releasing SDK packages."
---

# Azure TypeSpec Author

## Quick Reference

| Property      | Value                                                    |
| ------------- | -------------------------------------------------------- |
| **Services**  | Azure TypeSpec API Specifications (ARM & Data-plane)     |
| **Best For**  | Authoring, modifying, and troubleshooting `.tsp` files   |
| **Primary Scenario** | Adding a new API version for an Azure ARM service |

## When to Use This Skill

- Adding a new API version (preview or stable) to an ARM service
- Creating, modifying, or deleting content in `.tsp` files
- ARM resource definitions (tracked, proxy, extension, child resources)
- Resource operations (CRUD, PATCH, custom actions, async/LRO)
- Models, enums, unions, properties, decorators, and constraints
- Version-specific changes using versioning decorators

---

## Operating Principles

> **Non-negotiable** — all principles below apply to every invocation of this skill.

1. **This skill is MANDATORY for ALL `.tsp` file edits.** Any request that modifies, creates, or deletes content in a `.tsp` file MUST follow the full workflow. There are no "trivial" TypeSpec edits — even changing a single `?` (optional → required) can be a breaking change requiring versioning decorators.
2. **Do not edit any files until you have completed intake and determined the versioning scenario.**
3. **Make minimal, scoped edits** to satisfy the request. Avoid refactors unless explicitly asked.
4. **Follow the project's existing conventions** — naming patterns, file organization, decorator style, enum member naming.
5. **After edits, validate** by running `npx tsp compile .` and report results.
6. **Always provide references** from the embedded documentation that justify the recommended approach.

---

## Workflow Steps

> **All steps are MANDATORY. Do NOT skip any step.**

| Step | Name                                                  | Reference File / Tool                 | Gate                                    |
| ---- | ----------------------------------------------------- | ------------------------------------- | --------------------------------------- |
| 1    | [Intake & Clarification](#step-1-intake--clarification) | `references/intake-arm.md`          | All inputs collected + analysis displayed |
| 2    | [Determine Approach](#step-2-determine-approach)      | `references/arm-versioning-guide.md`  | Versioning scenario matched + plan ready |
| 3    | [Apply Changes](#step-3-apply-changes)                | `references/arm-versioning-operations.md` | Changes applied to `.tsp` files     |
| 4    | [Validate](#step-4-validate)                          | `npx tsp compile .`                   | Compilation passes                      |
| 5    | [Summarize](#step-5-summarize)                        | —                                     | Summary displayed to user               |
| 6    | [Next Steps](#step-6-next-steps)                      | `references/next-steps-arm.md`        | Follow-up actions presented             |

---

### Step 1: Intake & Clarification

Follow `references/intake-arm.md` to gather all required inputs.

**Key actions:**
1. Read the TypeSpec project files (`main.tsp`, `tspconfig.yaml`, imported `.tsp` files)
2. Identify existing API versions and the latest version type (preview/stable)
3. Determine what the user wants (new version, new feature, fix)
4. Collect the new version string and feature requirements

Do NOT proceed to Step 2 until all required inputs are collected **and** the analysis output has been displayed.

---

### Step 2: Determine Approach

Read `references/arm-versioning-guide.md` and match the user's scenario:

| Scenario | Guide Section |
|----------|--------------|
| Preview after Preview (no stable exists) | §1 |
| Preview after Preview (stable exists) | §2 |
| Preview after Stable | §3 |
| Stable after Preview | §4 |

Read `references/arm-versioning-operations.md` for the specific decorators and patterns needed.

**Present the plan to the user before making changes:**

```
Plan:
  Scenario: [Preview after Stable]
  Actions:
    1. Add version entry `v2025_06_01_preview: "2025-06-01-preview"` to Versions enum
    2. Add @previewVersion decorator to new entry
    3. Add @armCommonTypesVersion(v5) to new entry
    4. [Feature-specific actions...]

Proceed?
```

---

### Step 3: Apply Changes

Only after the user confirms the plan:

1. Follow the step-by-step instructions from the matched scenario in `arm-versioning-guide.md`
2. Use the decorator patterns from `arm-versioning-operations.md` for version-specific changes
3. Follow the project's existing conventions (file organization, naming, imports)
4. Add necessary imports if new files are created (update `main.tsp`)

**Key rules:**
- Use `@previewVersion` only on the last version enum entry
- Use `createOrReplace` templates (not `createOrUpdate`)
- Use `ArmCustomPatch` for PATCH operations
- Top-level tracked resources MUST have `listByResourceGroup` and `listBySubscription`
- When adding a whole new resource, put `@added` on the interface — no need on individual operations

---

### Step 4: Validate

Run compilation to validate changes:

```shell
cd <typespec-project-root>
npx tsp compile .
```

- If validation **passes** → proceed to Step 5
- If validation **fails** → read the error messages, fix with minimal changes, and re-validate
- Do NOT skip validation even if the change appears trivial

---

### Step 5: Summarize

Return the following to the user:

| Item                   | Detail                                                  |
| ---------------------- | ------------------------------------------------------- |
| **Scenario**           | Which versioning scenario was applied                   |
| **Files changed**      | List of modified/created files                          |
| **What changed**       | Brief description of changes and rationale              |
| **Validation results** | Pass/fail + key output                                  |
| **References**         | Links to relevant documentation sections used           |

---

### Step 6: Next Steps

Read the file `references/next-steps-arm.md` (using the read_file tool) and execute **ALL** of its instructions.

> **CRITICAL**: Do NOT end your turn without:
> 1. Checking examples are up to date
> 2. Asking the user what features they want to add to this version
> 3. Presenting scenario-specific follow-up actions

If the user requests additional features → **restart from Step 1** with the new feature request.

---

## Related Resources

| Resource                                                                           | Purpose                                              |
| ---------------------------------------------------------------------------------- | ---------------------------------------------------- |
| [`references/intake-arm.md`](references/intake-arm.md)                             | Step 1 — Intake and clarification                    |
| [`references/arm-versioning-guide.md`](references/arm-versioning-guide.md)         | Step 2 — ARM versioning scenarios and procedures     |
| [`references/arm-versioning-operations.md`](references/arm-versioning-operations.md) | Step 3 — Versioning decorators and code patterns   |
| [`references/next-steps-arm.md`](references/next-steps-arm.md)                     | Step 6 — Post-authoring follow-up actions            |
