# Azure TypeSpec Authoring

This plugin connects [GitHub Copilot CLI](https://github.com/github/copilot-cli) to the [Azure SDK MCP Server](https://github.com/nicktarlin/azure-sdk-mcp), letting you author, modify, and troubleshoot `.tsp` files for Azure ARM and data-plane API specifications directly from your development environment.

## Setup

### 1. Install GitHub Copilot CLI

Install the GitHub Copilot CLI by following the [official installation guide](https://docs.github.com/en/copilot/how-tos/copilot-cli).

### 2. Start an Interactive Session

```shell
copilot --log-dir .logs --log-level debug
```

### 3. Install the Plugin

In the GitHub Copilot CLI interactive Session

```
# Add the repo as a plugin marketplace
/plugin marketplace add haolingdong-msft/azure-typespec-authoring

# Install the plugin
/plugin install azure-typespec-authoring@azure-typespec-authoring
```

### 4. Verify Installation

```
/skills list
```

You should see `azure-typespec-author` in the list of available skills.

## Capabilities

The `azure-typespec-author` skill helps you work with TypeSpec API specifications in the `azure-rest-api-specs` repository:

- **API Versioning** — Add new preview or stable API versions, promote preview to stable
- **Resource Definitions** — Define tracked, proxy, extension, and child ARM resources
- **Resource Operations** — CRUD, PATCH, custom actions, async/long-running operations (LRO)
- **Type Definitions** — Models, enums, unions, properties, decorators, and constraints
- **Swagger Conversion** — Convert existing Swagger definitions to TypeSpec

## Example Usage

Navigate to your TypeSpec project path. e.g. <your path to spec repo>\specification\widget\ and ask GitHub Copilot CLI to:

- "Add a new preview API version 2026-01-01-preview for widget resource manager"
- "Add an ARM resource named Asset with CRUD operations"
- "Add a new stable API version 2026-01-01 for widget resource manager"

The skill will analyze your project, gather requirements, apply changes, and run validation automatically.

## Update

```
/plugin update azure-typespec-authoring@azure-typespec-authoring
```

## Uninstall

```
/plugin uninstall azure-typespec-authoring@azure-typespec-authoring
```

## Documentation

For more information, visit:

- [TypeSpec Documentation](https://typespec.io/)
- [Azure REST API Specs Repository](https://github.com/Azure/azure-rest-api-specs)
- [GitHub Copilot CLI Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)

