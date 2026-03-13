# Install Plugin for Use Outside the Spec Repo

This guide covers installing and using the `azure-typespec-author` plugin **outside** the [`azure-rest-api-specs`](https://github.com/Azure/azure-rest-api-specs) repository — for example, in a standalone TypeSpec project or a private service specification repo.

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **GitHub Copilot CLI** | Install via the [official guide](https://docs.github.com/en/copilot/how-tos/copilot-cli) |
| **PowerShell 7+** (Core) | Required by the MCP server bootstrap script. Install from [PowerShell GitHub](https://github.com/PowerShell/PowerShell) |

## Install

### Step 1 — Install the Plugin

```shell
# Add the repo as a plugin marketplace
copilot plugin marketplace add haolingdong-msft/azure-typespec-authoring

# Install the plugin
copilot plugin install azure-typespec-authoring@azure-typespec-authoring
```

### Step 2 — Verify Installation

```shell
copilot skills list
```

You should see `azure-typespec-author` in the list of available skills.

### Step 3 — Start an Interactive Session

Navigate to your TypeSpec project directory (it should contain `.tsp` files and a `tspconfig.yaml`):

```shell
cd /path/to/your/typespec-project
copilot
```

The first time you start a session, the plugin will automatically download the `azsdk` CLI tool. This is cached and reused on subsequent runs.

### Step 4 — Use the Skill

Enter your TypeSpec authoring prompt. For example:

```
Add a new ARM resource named Asset with CRUD operations
```

The skill will analyze your project, generate a plan, apply changes, and validate.

## Update

```shell
copilot plugin update azure-typespec-authoring@azure-typespec-authoring
```

## Uninstall

```shell
copilot plugin uninstall azure-typespec-authoring@azure-typespec-authoring
```

## Sample Prompts

Refer to the [sample prompts](https://gist.github.com/haolingdong-msft/070ee0bfc3aab9c6ea24b084ec06a734#sample-prompts) for example usage scenarios.
