---
name: azure-typespec-author
license: MIT
metadata:
  version: "0.0.1"
description: "Author or modify Azure TypeSpec API specifications in the azure-rest-api-specs repository. USE FOR: Any task that creates, modifies, or troubleshoots .tsp files or TypeSpec API specifications — including but not limited to API versioning (e.g. add new preview version, add new stable version), ARM or data-plane resource definitions (tracked, proxy, extension, child resources), resource operations (CRUD, PATCH, custom actions, async/LRO), models, enums, unions, properties, decorators, constraints, and swagger-to-TypeSpec conversion. DO NOT USE FOR: SDK generation from TypeSpec, releasing SDK packages, single MCP tool calls that do not require multi-step workflows. TOOLS/COMMANDS: azsdk_typespec_generate_authoring_plan, azsdk_run_typespec_validation"
compatibility: >-
  Requires: azure-sdk-mcp server with azsdk_typespec_generate_authoring_plan and azsdk_run_typespec_validation tools.
---

# Azure TypeSpec Author

## MCP Prerequisites

Requires `azure-sdk-mcp` server with TypeSpec authoring and validation tools.

## MCP Tools

| Tool                                     | Purpose                              |
| ---------------------------------------- | ------------------------------------ |
| `azsdk_typespec_generate_authoring_plan` | Generate grounded authoring plan     |
| `azsdk_run_typespec_validation`          | Validate TypeSpec compilation + lint |

## Principles

1. **Mandatory for ALL `.tsp` edits** — even a single `?` change can be breaking.
2. **Minimal, scoped edits** — only change what the request requires.
3. **Always validate** — run `azsdk_run_typespec_validation` after every edit.
4. **Always cite references** — provide links that justify the approach.

## Steps

All tasks follow a 5-step workflow. Steps 2–3 branch by task type (API versioning vs general authoring).

1. **Analyze Project** — Follow the [project analysis guide](references/analyze-project.md) to collect project context. Route: *API versioning* → ARM Versioning Guide, *general authoring* → [intake guide](references/intake.md).
2. **Intake & Clarification** — *API Versioning:* identify scenario from [ARM Versioning Guide][vg] ([preview→preview][pp], [preview→stable][sp], [stable→preview][ps], [stable→stable][ss]) and gather required information from user. *General Authoring:* follow [intake guide](references/intake.md).
3. **Retrieve Solution** — *API Versioning:* follow the scenario guide from Step 2 (no MCP call needed). *General Authoring:* invoke `azsdk_typespec_generate_authoring_plan` with request, context from Steps 1–2, and project root path. Do not proceed without a grounded plan.
4. **Apply Changes** — Confirm uncertainties with user, make minimal `.tsp` edits, prefer official templates from retrieved context.
5. **Validate** — Follow the [validation guide](references/validation.md).

## References
[vg]: https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/01-about-versioning/
[pp]: https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/02-preview-after-preview/
[sp]: https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/03-stable-after-preview/
[ps]: https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/04-preview-after-stable/
[ss]: https://azure.github.io/typespec-azure/docs/howtos/versioning/arm/05-stable-after-stable/


