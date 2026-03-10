# Azure TypeSpec Authoring

This plugin connects [GitHub Copilot CLI](https://github.com/github/copilot-cli) to the [Azure SDK MCP Server](https://github.com/nicktarlin/azure-sdk-mcp), letting you author, modify, and troubleshoot `.tsp` files for Azure ARM and data-plane API specifications directly from your development environment.

## Setup

### 1. Install GitHub Copilot CLI

Install the GitHub Copilot CLI by following the [official installation guide](https://docs.github.com/en/copilot/how-tos/copilot-cli).

### 2. Install the Plugin

```shell
# Add the repo as a plugin marketplace
copilot plugin marketplace add haolingdong-msft/azure-typespec-authoring

# Install the plugin
copilot plugin install azure-typespec-authoring@azure-typespec-authoring
```

### 3. Verify Installation

```shell
copilot skills list
```

You should see `azure-typespec-author` in the list of available skills.

### 4. Start an Interactive Session

Navigate to your TypeSpec project path (e.g. `<your path to spec repo>\specification\widget\`) and start a session:

```shell
copilot --log-dir .logs --log-level debug
```

### 5. Input prompts


## Sample promtps:

### API Versioning

- "Add a new preview API version 2026-01-01-preview for widget resource manager"
- "Add a new stable API version 2026-01-01 for widget resource manager"

### Resource Definitions

- "Add an ARM resource named Asset with CRUD operations"
- "Add a child resource named Component under the Asset resource"
- "Add a proxy resource named Config under the Asset resource"

### Resource Operations

- "Add a custom action restartAsset to the Asset resource"
- "Add an async/LRO operation to export data from the Asset resource"
- "Add a PATCH operation to the Asset resource"

### Models & Types

- "Add an enum named AssetStatus with values Active, Inactive, and Deprecated"
- "Add a new property tags to the Asset resource"

## Capabilities

The `azure-typespec-author` skill helps you work with TypeSpec API specifications in the `azure-rest-api-specs` repository:

- **API Versioning** — Add new preview or stable API versions, promote preview to stable
- **Resource Definitions** — Define tracked, proxy, extension, and child ARM resources
- **Resource Operations** — CRUD, PATCH, custom actions, async/long-running operations (LRO)
- **Type Definitions** — Models, enums, unions, properties, decorators, and constraints

## Update

```shell
copilot plugin update azure-typespec-authoring@azure-typespec-authoring
```

## Uninstall

```shell
copilot plugin uninstall azure-typespec-authoring@azure-typespec-authoring
```

## Documentation

For more information, visit:

- [TypeSpec Documentation](https://typespec.io/)
- [Azure REST API Specs Repository](https://github.com/Azure/azure-rest-api-specs)
- [GitHub Copilot CLI Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)

