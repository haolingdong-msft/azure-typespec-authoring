# TypeSpec ARM Authoring — Intake & Clarification

Complete all sub-steps to collect required information before proceeding to implementation.

| Sub-step                                                     | Goal                                  | Mandatory? |
| ------------------------------------------------------------ | ------------------------------------- | ---------- |
| [1.1 Analyze Project](#step-11-analyze-the-typespec-project) | Gather project inputs                 | Yes        |
| [1.2 Determine Scenario](#step-12-determine-versioning-scenario) | Match to ARM versioning scenario  | Yes        |
| [1.3 Display Results](#step-13-display-analysis-results)     | Show analysis summary to user         | Yes        |
| [1.4 Collect Feature Requirements](#step-14-collect-feature-requirements) | Ask what to add in this version | Yes (Cases 1-4) |
| [1.5 Confirm & Proceed](#step-15-summary-and-confirmation)   | Confirm collected information         | Yes        |

---

## Step 1.1: Analyze the TypeSpec Project

> **Prerequisite**: ALWAYS complete this step before asking any other questions.

Read the project files and collect the following inputs. Ask **up to 4 concise questions** for any that are missing:

| #   | Input                          | How to Find                                                          |
| --- | ------------------------------ | -------------------------------------------------------------------- |
| 1   | **TypeSpec project root**      | Directory containing `main.tsp` and `tspconfig.yaml`                |
| 2   | **Namespace**                  | `namespace Microsoft.XXX` in `main.tsp`                             |
| 3   | **Existing API versions**      | `Versions` enum in `main.tsp` — list all entries with their values  |
| 4   | **Latest version**             | Last entry in the `Versions` enum                                   |
| 5   | **Latest version type**        | Has `@previewVersion` decorator → preview; otherwise → stable       |
| 6   | **Has stable versions?**       | Any entry in `Versions` enum without `@previewVersion` and without `-preview` in value |
| 7   | **New version string**         | Ask user (format: `YYYY-MM-DD-preview` or `YYYY-MM-DD`)            |
| 8   | **New version type**           | Ends with `-preview` → preview; otherwise → stable                  |
| 9   | **Intent**                     | add version / modify existing / fix issue                           |

### Files to Read

Read these files from the TypeSpec project to understand its current state:

- `main.tsp` — namespace, imports, `Versions` enum, `@versioned` decorator
- `tspconfig.yaml` — emitter configuration
- All imported `.tsp` files — existing resources, models, operations, interfaces
- `examples/` directory — existing example files and which versions they cover

---

## Step 1.2: Determine Versioning Scenario

Based on the inputs from Step 1.1, determine which ARM versioning scenario applies:

| Scenario | Condition | Description | Reference |
| -------- | --------- | ----------- | --------- |
| **Preview after Preview (no stable)** | Latest is preview, no stable versions exist, new version is preview | Rename the latest preview version everywhere. Remove all versioning decorators. | `arm-versioning-guide.md` §1 |
| **Preview after Preview (stable exists)** | Latest is preview, stable versions exist, new version is preview | Rename the latest preview version and its decorators to the new version. | `arm-versioning-guide.md` §2 |
| **Preview after Stable** | Latest is stable, new version is preview | Add a new preview entry to the `Versions` enum with `@previewVersion`. Mark all new changes with decorators referencing this version. | `arm-versioning-guide.md` §3 |
| **Stable after Preview** | Latest is preview, new version is stable | Promote preview features to stable. Add new stable entry. Remove `@previewVersion` from the promoted version. | `arm-versioning-guide.md` §4 |

> If the user's request doesn't match any scenario, proceed directly to Step 2 (Retrieve Solution) using the project analysis and user request.

---

## Step 1.3: Display Analysis Results

> **MANDATORY**: Display this output before proceeding. Do NOT skip.

```
╔══════════════════════════════════════════════════╗
║          TypeSpec Project Analysis               ║
╠══════════════════════════════════════════════════╣
║ Project Root:    /path/to/project                ║
║ Namespace:       Microsoft.XXX                   ║
║ API Versions:    2024-01-01 (stable)             ║
║                  2024-06-01-preview (preview)    ║
║ Latest Version:  2024-06-01-preview (preview)    ║
║ Has Stable:      Yes                             ║
║ New Version:     2025-01-01-preview (preview)    ║
║ Scenario:        Preview after Preview           ║
╚══════════════════════════════════════════════════╝
```

---

## Step 1.4: Collect Feature Requirements

> Only for versioning scenarios (Cases 1-4). Skip if user request is a specific fix/modification.

Ask the user:

1. **What features do you want to add in this version?** Examples:
   - New resources (e.g., "add a StorageAccount resource")
   - New properties on existing resources (e.g., "add an `encryption` property to Widget")
   - New operations (e.g., "add a custom `restart` action on VirtualMachine")
   - Modified behavior (e.g., "make `location` optional", "convert PUT from sync to async")
   - Removed features (e.g., "remove the `legacyMode` property")

2. If the user says "no new features" or "just the version bump", proceed with only the version change.

3. If the user provides features, collect enough detail for each:
   - For new resources: name, properties, parent resource (if child), operations needed
   - For new properties: target model, property name, type, required/optional, description
   - For new operations: target resource/interface, operation type, sync/async
   - For removals: what to remove and confirmation of breaking change awareness

---

## Step 1.5: Summary and Confirmation

Display collected information and wait for user confirmation:

```
╔══════════════════════════════════════════════════╗
║          Ready to Proceed                        ║
╠══════════════════════════════════════════════════╣
║ Scenario:        Preview after Preview           ║
║ Namespace:       Microsoft.Widget                ║
║ Project Path:    /specification/widget/...       ║
║ Current Latest:  2024-06-01-preview (preview)    ║
║ New Version:     2025-01-01-preview (preview)    ║
║ Features:                                        ║
║   1. Add `encryption` property to Widget         ║
║   2. Add new StorageConfig child resource        ║
╚══════════════════════════════════════════════════╝

Proceed? (yes / no / modify)
```

Once confirmed, proceed to Step 2: Apply Changes.
