# Azure TypeSpec Authoring 

A skill that helps to author or modify Azure TypeSpec API specifications in the azure-rest-api-specs repository. USE FOR: Any task that creates, modifies, or troubleshoots .tsp files or TypeSpec API specifications — including but not limited to API versioning, ARM or data-plane resource definitions (tracked, proxy, extension, child resources), resource operations (CRUD, PATCH, custom actions, async/LRO), models, enums, unions, properties, decorators, constraints.

## Quick Start

### Prerequisite

Open from azure-rest-api-spec root folder.

### Use Published Alpha Version

👉 Follow the [Quick Start Guide](https://gist.github.com/haolingdong-msft/070ee0bfc3aab9c6ea24b084ec06a734#file-typespec-authoring-agent-quick-start-md).

⚠️ Please note: This version is lack of support for API version evolution and Azure data-plane scenarios. 


### Use Latest Dev Version

✅ This version supports basic Azure data-plane and API version evolution scenarios. 

Check out the branch [update-typespec-author-skill](https://github.com/Azure/azure-rest-api-specs/tree/update-typespec-author-skill) and author TypeSpec from there. 

Other steps are the same with 'Use Published Alpha Version'.

## Sample promtps:

Refer [here](https://gist.github.com/haolingdong-msft/070ee0bfc3aab9c6ea24b084ec06a734#sample-prompts).


## Author TypeSpec Outside of Spec Repo

See the [Plugin Install Guide](docs/install-plugin-outside-spec-repo.md) for how to install and use the plugin outside the `azure-rest-api-specs` repository.

## Documentation

For more information, visit:

- [TypeSpec Documentation](https://typespec.io/)
- [Azure REST API Specs Repository](https://github.com/Azure/azure-rest-api-specs)
- [GitHub Copilot CLI Plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)

