# TypeSpec ARM Authoring — Next Steps

After authoring changes are complete and validation passes, execute the follow-up steps below.

---

## For All Versioning Scenarios

### 1. Check Examples

Verify that example files match the new API version:

- **New version added**: Create a new `examples/<new-version>/` folder with example JSON files for all operations
- **Version renamed** (preview-after-preview): Rename the examples folder and update `api-version` in all example files
- **New operations/resources added**: Add corresponding example files

If examples are missing or outdated → **offer to help create/update them**.

### 2. Proactively Ask About Features

> **MANDATORY for versioning scenarios**: Do NOT end the conversation without asking this.

Ask the user:

```
The new version has been created successfully. What features would you like to add?

Common additions:
  • New resource types (e.g., child resources, extension resources)
  • New properties on existing resources
  • New operations (custom actions, list operations)
  • Modified behavior (make property optional/required, change type)
  • New enum values

Would you like to add any features, or are you done with this version?
```

If the user provides a feature request → **restart the full workflow from Step 1** to implement it.

If the user says done → proceed to step 3.

### 3. Compilation Reminder

Remind the user to compile and verify:

```
Before submitting a PR, make sure to:
  1. Run `npx tsp compile .` from the TypeSpec project root
  2. Check the generated OpenAPI output matches expectations
  3. Update README.md if needed
```

---

## Scenario-Specific Follow-ups

### Preview after Preview

- **Old OpenAPI cleanup**: Ask if the old preview version's OpenAPI files should be removed
  - If the old preview is being retired → remove the OpenAPI directory
  - If RPaaS validation or ARM registration still needs it → keep it
- **Update README.md**: Remove old preview entry if retiring, add new preview entry

### Preview after Stable

- No old version cleanup needed (the stable version remains)
- **Update README.md**: Add the new preview version entry

### Stable after Preview

- **Breaking change review**: Compare changes against the previous stable version. Flag:
  - Removed or renamed properties, resources, or operations
  - Changed property types
  - Changed required ↔ optional
  - Removed enum members
  - Changed response codes
  
  > Breaking changes from previous **preview** → new stable: **expected**, no review needed.
  > Breaking changes from previous **stable** → new stable: **requires review** and user confirmation.

- **Preview feature promotion**: List all features from the latest preview. Ask user:
  - Promote all features to stable?
  - Exclude any features (keep them preview-only)?

- **Old preview cleanup**: If all preview features are promoted, the preview version can be removed.

---

## References

- [ARM Versioning Guide](arm-versioning-guide.md)
- [ARM Versioning Operations](arm-versioning-operations.md)
- [Azure Retirement Policy](https://aka.ms/AzureRetirementPolicy)
