# azure-typespec-authoring

A [GitHub Copilot CLI plugin](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating) for authoring Azure TypeSpec API specifications.

## What It Does

This plugin provides the `azure-typespec-author` skill which helps you author, modify, and troubleshoot `.tsp` files for Azure ARM and data-plane API specifications — including API versioning, resource definitions, CRUD operations, models, enums, and more.

## Plugin Structure

```
.github/
└── plugin/
    └── marketplace.json                               # Marketplace manifest
plugin/
├── plugin.json                                    # Plugin manifest
├── .mcp.json                                      # MCP server configuration
└── skills/
    └── azure-typespec-author/
        ├── SKILL.md                               # Skill definition
        └── references/
            ├── intake-arm.md                      # Step 1 — Intake & clarification
            └── next-steps-arm.md                  # Step 6 — Follow-up actions
```

## Installation

### Option 1: Install from Marketplace (Recommended)

Register the marketplace first, then install the plugin:

```shell
copilot plugin marketplace add haolingdong-msft/azure-typespec-authoring
copilot plugin install azure-typespec-authoring
```

Or in an interactive session:

```
/plugin marketplace add haolingdong-msft/azure-typespec-authoring
/plugin install azure-typespec-authoring
```

### Option 2: Install directly from GitHub

```shell
copilot plugin install haolingdong-msft/azure-typespec-authoring:plugin
```

Or in an interactive session:

```
/plugin install haolingdong-msft/azure-typespec-authoring:plugin
```

## Update

```shell
copilot plugin update azure-typespec-authoring
```

Or in an interactive session:

```
/plugin update azure-typespec-authoring
```

## Verify

In an interactive session:

```
/skills list
```

## Uninstall

```shell
copilot plugin uninstall azure-typespec-authoring
```

Or in an interactive session:

```
/plugin uninstall azure-typespec-authoring
```

