# ARM Versioning Operations

This document describes how to use TypeSpec versioning decorators to make version-specific changes to ARM API specifications.

## Import Requirements

Versioning decorators require these imports:

```typespec
import "@typespec/versioning";
using TypeSpec.Versioning;
```

These are typically already present in ARM TypeSpec projects.

---

## Versioning Decorators Reference

### `@added(version)`

Introduces an element in a specific version and all subsequent versions.

**Use for**: Adding new models, properties, operations, parameters, enum members.

```typespec
// Add a model in a specific version
@added(Versions.v2025_06_01_preview)
model StorageConfig {
  accountName: string;
  containerName?: string;
}

// Add a property to an existing model
model WidgetProperties {
  name: string;

  @added(Versions.v2025_06_01_preview)
  encryption?: EncryptionSettings;
}

// Add an operation to an interface
@armResourceOperations
interface Widgets {
  get is ArmResourceRead<Widget>;

  @added(Versions.v2025_06_01_preview)
  restart is ArmResourceActionAsync<Widget, RestartRequest, RestartResponse>;
}

// Add a parameter to an operation
@armResourceOperations
interface Widgets {
  listBySubscription is ArmListBySubscription<
    Widget,
    Parameters = {
      @added(Versions.v2025_06_01_preview)
      @query("orderBy")
      orderBy?: string;
    }
  >;
}
```

---

### `@removed(version)`

Removes an element starting from a specific version. The element will NOT be present in that version or later.

**Use for**: Removing models, properties, operations, enum members.

```typespec
// Remove a property
model WidgetProperties {
  name: string;

  @removed(Versions.v2025_06_01_preview)
  legacyMode?: boolean;
}

// Remove an operation
@armResourceOperations
interface Widgets {
  get is ArmResourceRead<Widget>;

  @removed(Versions.v2025_06_01_preview)
  deprecatedAction is ArmResourceActionSync<Widget, void, void>;
}

// Remove a model entirely
@removed(Versions.v2025_06_01_preview)
model LegacyConfig {
  setting: string;
}
```

---

### `@renamedFrom(version, oldName)`

Renames an element starting from a specific version. Keeps the old name for previous versions.

**Use for**: Renaming properties, models, operations, enum members.

```typespec
// Rename a property
model WidgetProperties {
  @renamedFrom(Versions.v2025_06_01_preview, "state")
  status?: string;
}

// Rename a model
@renamedFrom(Versions.v2025_06_01_preview, "WidgetConfig")
model WidgetConfiguration {
  setting: string;
}
```

---

### `@madeOptional(version)`

Makes a previously required property optional starting from a specific version.

```typespec
model WidgetProperties {
  @madeOptional(Versions.v2025_06_01_preview)
  location?: string;
}
```

> **Note**: The property declaration must have `?` (optional marker). The decorator records that it was required before that version.

---

### `@madeRequired(version)`

Makes a previously optional property required starting from a specific version.

```typespec
model WidgetProperties {
  @madeRequired(Versions.v2025_06_01_preview)
  sku: string;
}
```

> **Note**: The property declaration must NOT have `?`. The decorator records that it was optional before that version.

---

### `@typeChangedFrom(version, oldType)`

Records that a property's type changed in a specific version.

```typespec
model WidgetProperties {
  @typeChangedFrom(Versions.v2025_06_01_preview, string)
  priority: int32;
}
```

---

## Common Patterns

### Adding a New Resource in a Specific Version

When adding an entirely new resource that should only appear in a specific version and later, use `@added` on the **interface** (not individual operations):

```typespec
// New resource model (properties don't need @added if the whole resource is new)
@parentResource(Widget)
model StorageConfig
  is Azure.ResourceManager.ProxyResource<StorageConfigProperties, false> {
  ...ResourceNameParameter<
    Resource = StorageConfig,
    KeyName = "configName",
    SegmentName = "storageConfigs",
    NamePattern = ""
  >;
}

model StorageConfigProperties {
  @visibility(Lifecycle.Read)
  provisioningState?: ProvisioningState;

  accountName: string;
  containerName?: string;
}

// Mark the interface with @added so ALL operations appear in that version
@armResourceOperations
@added(Versions.v2025_06_01_preview)
interface StorageConfigs {
  get is ArmResourceRead<StorageConfig>;
  createOrUpdate is ArmResourceCreateOrReplaceAsync<StorageConfig>;
  delete is ArmResourceDeleteWithoutOkAsync<StorageConfig>;
  list is ArmResourceListByParent<StorageConfig>;
}
```

**Key point**: When the `@added` decorator is on the interface, it covers all operations within — no need to add `@added` to each operation individually.

### Adding a New Property to an Existing Model

```typespec
model WidgetProperties {
  // Existing properties (no decorators needed)
  name: string;
  location: string;

  // New property in the preview version
  @added(Versions.v2025_06_01_preview)
  encryption?: EncryptionSettings;
}
```

### Adding a New Operation to an Existing Interface

```typespec
@armResourceOperations
interface Widgets {
  // Existing operations (no decorators needed)
  get is ArmResourceRead<Widget>;
  createOrUpdate is ArmResourceCreateOrReplaceAsync<Widget>;

  // New operation in the preview version
  @added(Versions.v2025_06_01_preview)
  restart is ArmResourceActionAsync<Widget, RestartRequest, RestartResponse>;
}
```

---

## Complex Scenarios

### Changing a Decorator on an Existing Property

When a property's decorator changes between versions (e.g., visibility change), use `@removed` + `@added` + `@renamedFrom`:

```typespec
model WidgetProperties {
  // v1: read-only
  @removed(Versions.v2025_06_01_preview)
  @visibility(Lifecycle.Read)
  @renamedFrom(Versions.v2025_06_01_preview, "experience")
  oldExperience: string;

  // v2: read + create
  @added(Versions.v2025_06_01_preview)
  @visibility(Lifecycle.Read, Lifecycle.Create)
  experience: string;
}
```

### Converting Sync to Async Operation

```typespec
@armResourceOperations
interface Widgets {
  // v1: sync
  @removed(Versions.v2025_06_01_preview)
  @renamedFrom(Versions.v2025_06_01_preview, "createOrUpdate")
  @sharedRoute
  createOrUpdateV1 is ArmResourceCreateOrReplaceSync<Widget>;

  // v2: async
  @added(Versions.v2025_06_01_preview)
  @sharedRoute
  createOrUpdate is ArmResourceCreateOrReplaceAsync<Widget>;
}
```

**Key**: Use `@sharedRoute` on both so they share the same HTTP route.

### Making a Parameter Optional and Adding a New Parameter

```typespec
@armResourceOperations
interface Widgets {
  listBySubscription is ArmListBySubscription<
    Widget,
    Parameters = {
      @madeOptional(Versions.v2025_06_01_preview)
      @header
      location?: string;

      @added(Versions.v2025_06_01_preview)
      @query("order-by")
      orderBy?: string;
    }
  >;
}
```

### Versioning a Pattern Change

**If the pattern change applies to all versions** (reflects actual service behavior):

```typespec
model WidgetProperties {
  @pattern("^[a-zA-Z]+$")  // Updated pattern
  name: string;
}
```

**If different patterns per version** (very unlikely):

```typespec
model WidgetProperties {
  @removed(Versions.v2025_06_01_preview)
  @pattern("^[a-z]+$")
  @renamedFrom(Versions.v2025_06_01_preview, "name")
  oldName: string;

  @added(Versions.v2025_06_01_preview)
  @pattern("^[a-zA-Z]+$")
  name: string;
}
```

---

## ARM Resource Operation Templates

When adding operations, use these standard ARM templates:

| Operation | Template | Default Sync/Async |
|-----------|----------|-------------------|
| GET | `ArmResourceRead<Resource>` | Sync (always) |
| PUT (create/replace) | `ArmResourceCreateOrReplaceAsync<Resource>` | Async (LRO) |
| PATCH | `ArmCustomPatchSync<Resource, PatchModel>` | Sync |
| DELETE | `ArmResourceDeleteWithoutOkAsync<Resource>` | Async (LRO) |
| List by parent | `ArmResourceListByParent<Resource>` | Sync (always) |
| List by resource group | `ArmListBySubscription<Resource>` | Sync (always) |
| Custom action (sync) | `ArmResourceActionSync<Resource, Request, Response>` | Sync |
| Custom action (async) | `ArmResourceActionAsync<Resource, Request, Response>` | Async (LRO) |

> **Important**: Use `createOrReplace` templates (not `createOrUpdate`). Use `ArmCustomPatch` for PATCH operations.

---

## References

- [TypeSpec Versioning Decorators](https://typespec.io/docs/libraries/versioning/reference/decorators/)
- [ARM Versioning Operations](https://azure.github.io/typespec-azure/docs/howtos/arm/versioning/)
- [Getting Started with Versioning](https://azure.github.io/typespec-azure/docs/getstarted/versioning/)
