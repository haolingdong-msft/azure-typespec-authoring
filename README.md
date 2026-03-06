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

First, register the marketplace in an interactive Copilot CLI session:

```
/plugin marketplace add haolingdong-msft/azure-typespec-authoring
```

Then install the plugin from the marketplace:

```
/plugin install azure-typespec-authoring@azure-typespec-authoring
```

## Update

```
/plugin update azure-typespec-authoring
```

## Verify


in an interactive session:

```
/skills list
```

## Uninstall

```shell
copilot plugin uninstall azure-typespec-authoring
```