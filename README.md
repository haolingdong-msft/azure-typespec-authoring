# Azure TypeSpec Authoring 

A skill that helps to author or modify Azure TypeSpec API specifications in the azure-rest-api-specs repository. USE FOR: Any task that creates, modifies, or troubleshoots .tsp files or TypeSpec API specifications — including but not limited to API versioning, ARM or data-plane resource definitions (tracked, proxy, extension, child resources), resource operations (CRUD, PATCH, custom actions, async/LRO), models, enums, unions, properties, decorators, constraints.

## Quick Start

### 🚀 Get Started in 1 Minute with Published Alpha Version

👉 Follow the [Quick Start Guide](https://gist.github.com/haolingdong-msft/070ee0bfc3aab9c6ea24b084ec06a734#file-typespec-authoring-agent-quick-start-md).

⚠️ Please note: This version is lack of support for API version evolution and Azure data-plane scenarios. 


### Want to use Latest Dev Version?

✅ This version supports basic Azure data-plane and API version evolution scenarios. 

📖 **Using outside the spec repo?** See the [Plugin Install Guide](docs/install-plugin-outside-spec-repo.md) for detailed instructions on prerequisites, how it works, and troubleshooting.

### 1. Install GitHub Copilot CLI

Install the GitHub Copilot CLI by following the [official installation guide](https://docs.github.com/en/copilot/how-tos/copilot-cli).

### 2. Add the Skill

```shell
npx skills add https://github.com/haolingdong-msft/azure-typespec-authoring/tree/main/plugin/skills
```

⚠️
**If using inside the `azure-rest-api-specs` repo:** The project-level `azure-typespec-author` skill **always overrides** the plugin version. You **must** rename the project-level skill file to disable it, otherwise the latest dev version skill will be silently ignored:

```shell
mv .github/skills/azure-typespec-author/SKILL.md .github/skills/azure-typespec-author/SKILL.md.disabled
```


### 3. Start an Interactive Session

Navigate to your TypeSpec project path (e.g. `<your path to spec repo>\specification\widget\`) and start a session:

```shell
copilot
```

### 4. Input prompts


## Sample promtps:

Refer [here](https://gist.github.com/haolingdong-msft/070ee0bfc3aab9c6ea24b084ec06a734#sample-prompts).

## Capabilities

The `azure-typespec-author` skill helps you work with TypeSpec API specifications in the `azure-rest-api-specs` repository:

- **API Versioning** — Add new preview or stable API versions, promote preview to stable
- **Resource Definitions** — Define tracked, proxy, extension, and child ARM resources
- **Resource Operations** — CRUD, PATCH, custom actions, async/long-running operations (LRO)
- **Type Definitions** — Models, enums, unions, properties, decorators, and constraints

## Author TypeSpec Outside of Spec Repo

See the [Plugin Install Guide](docs/install-plugin-outside-spec-repo.md) for how to install and use the plugin outside the `azure-rest-api-specs` repository.

## Update

Re-run the add command to update to the latest version:

```shell
npx skills add haolingdong-msft/azure-typespec-authoring --skill azure-typespec-author
```

## Uninstall

```shell
npx skills remove azure-typespec-author
```

## Documentation

For more information, visit:

- [TypeSpec Documentation](https://typespec.io/)
- [Azure REST API Specs Repository](https://github.com/Azure/azure-rest-api-specs)
- [GitHub Copilot CLI Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)

