# ARM Versioning Guide

This document describes the ARM versioning scenarios for Azure TypeSpec API specifications. Use this guide to determine how to add or modify API versions.

## Core Rules

1. **Single active preview**: TypeSpec specs should only have a **single active preview** version at any time.
2. **`@previewVersion` decorator**: Only the **last** entry in the `Versions` enum may be marked with `@previewVersion`.
3. **Preview version format**: `YYYY-MM-DD-preview` (e.g., `2025-06-01-preview`)
4. **Stable version format**: `YYYY-MM-DD` (e.g., `2025-06-01`)
5. **`@armCommonTypesVersion`**: Each version entry should specify the ARM common types version (typically `v5`).
6. **Version enum naming**: Use format `v{YYYY}_{MM}_{DD}_preview` for preview, `v{YYYY}_{MM}_{DD}` for stable (matching the service's existing convention).

---

## §1 Preview after Preview — No Stable Versions Exist

**When**: The spec has only preview version(s) and no stable versions. You want a new preview.

**Strategy**: Simply rename the latest preview to the new version. No versioning decorators needed.

### Steps

1. **Rename the version** in the `Versions` enum:

```typespec
// Before
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-06-01-preview`,
}

// After
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-12-01-preview`,
}
```

2. **Remove any versioning decorators** (`@added`, `@removed`, etc.) since there's no stable baseline to diff against.

3. **Make API changes directly** — add/remove/modify types without versioning decorators.

4. **Rename the examples folder**:
   - Rename `examples/2025-06-01-preview` → `examples/2025-12-01-preview`
   - Update `api-version` values inside all example JSON files

5. **Update README.md** to reference the new version.

6. **Remove old OpenAPI** if the old preview is being retired (check with user).

---

## §2 Preview after Preview — Stable Versions Exist

**When**: The latest version is preview, stable versions exist before it, and you want a new preview.

**Strategy**: Rename the latest preview version (and all its decorators) to the new preview version.

### Steps

1. **Rename the version** in the `Versions` enum — change both the enum member name and value:

```typespec
// Before
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-06-01-preview`,
}

// After
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-12-01-preview`,
}
```

2. **Rename all references** to the old preview version throughout the spec:
   - Use find-and-replace: `Versions.v2025_06_01_preview` → `Versions.v2025_12_01_preview` (or whatever naming convention the project uses)
   - This includes all `@added(Versions.xxx)`, `@removed(Versions.xxx)`, `@renamedFrom(Versions.xxx, ...)`, etc.

3. **Adjust decorators for API changes** between old and new preview:
   - Feature **added in old preview but NOT in new preview**: remove the type/property entirely (delete the code and its `@added` decorator)
   - Feature **removed in old preview but present in new preview**: remove the `@removed` decorator
   - Feature **renamed in old preview**: if rename no longer applies, remove the `@renamedFrom` decorator and revert to the original name
   - **New features** in this preview: add with `@added(Versions.v2025_12_01_preview)`

4. **Rename the examples folder** and update `api-version` values.

5. **Update README.md** for the new version.

6. **Remove old OpenAPI** if old preview is being retired.

---

## §3 Preview after Stable

**When**: The latest version is stable, and you want to add a new preview.

**Strategy**: Add a new version entry at the end of the `Versions` enum with `@previewVersion`.

### Steps

1. **Add a new version entry** at the end of the `Versions` enum:

```typespec
// Before
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,
}

// After
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-06-01-preview`,
}
```

2. **Mark all new changes** with versioning decorators referencing the new preview version:
   - New types/resources: `@added(Versions.v2025_06_01_preview)`
   - New properties: `@added(Versions.v2025_06_01_preview)`
   - New operations: `@added(Versions.v2025_06_01_preview)`
   - Removed items: `@removed(Versions.v2025_06_01_preview)`
   - See `arm-versioning-operations.md` for detailed decorator usage.

3. **Create a new examples folder**: `examples/2025-06-01-preview/` with examples for all operations in this version.

4. **Update README.md** with the new version entry.

---

## §4 Stable after Preview

**When**: The latest version is preview, and you want to promote it to a new stable version.

**Strategy**: Add a new stable version entry. Migrate preview features to stable by updating their `@added` decorator versions.

### Steps

1. **Add the new stable version** to the `Versions` enum (before the preview if a preview will remain, or as the last entry if removing the preview):

```typespec
// If removing the preview entirely
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2025-06-01`,
}

// If keeping a preview for features not yet GA
enum Versions {
  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2024-01-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  `2025-06-01`,

  @armCommonTypesVersion(Azure.ResourceManager.CommonTypes.Versions.v5)
  @previewVersion
  `2025-09-01-preview`,
}
```

2. **Migrate promoted features**: For each feature being promoted from preview to stable:
   - Change `@added(Versions.v_preview)` to `@added(Versions.v_stable)` (the new stable version)

3. **Handle features NOT promoted**: If some features remain preview-only:
   - Keep their `@added` decorators pointing to the preview version
   - Rename the preview version to a new preview if needed

4. **Breaking change review**: Compare against the previous stable version. Flag:
   - Removed/renamed properties or resources
   - Changed types
   - Changed required/optional
   - Removed operations or enum members
   - Changed response codes

   > Breaking changes from preview → stable are **expected** and do not require review.
   > Breaking changes from stable → stable **require review**.

5. **Create examples** for the new stable version.

6. **Update README.md**.

---

## Version Enum Naming Conventions

Projects in `azure-rest-api-specs` use different naming conventions for version enum members. **Always follow the existing convention in the project**:

| Convention | Example |
|-----------|---------|
| Backtick-quoted values | `` `2025-06-01-preview` `` |
| Identifier with value | `v2025_06_01_preview: "2025-06-01-preview"` |
| Short identifiers | `v1`, `v2`, `v3` (rare in real specs) |

### Common Types Version

Each version should specify `@armCommonTypesVersion`. Common values:

| Common Types Version | Usage |
|---------------------|-------|
| `v3` | Older specs |
| `v4` | Most current specs |
| `v5` | Latest specs |

**Follow the existing convention** in the project — use the same common types version as other entries unless there's a specific reason to change.

---

## References

- [About ARM API Versioning in TypeSpec](https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/01-about-versioning/)
- [Preview after Preview](https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/02-preview-after-preview/)
- [Stable after Preview](https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/03-stable-after-preview/)
- [Preview after Stable](https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/04-preview-after-stable/)
- [Stable after Stable](https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/05-stable-after-stable/)
- [Preview Version Rules](https://azure.github.io/typespec-azure/docs/howtos/versioning/01-preview-version/)
- [ARM Versioning Operations](https://azure.github.io/typespec-azure/docs/howtos/arm/versioning/)
