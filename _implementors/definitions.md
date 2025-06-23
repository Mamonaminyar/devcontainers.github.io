---
layout: implementors
title:  "Common Terms and Definitions"
shortTitle: "Definitions"
author: Microsoft
index: 10
---

This page provides definitions for common terms used throughout the Development Container Specification and related documentation. These definitions are intended to provide clarity and consistency in understanding dev container concepts.

## <a href="#core-concepts" name="core-concepts" class="anchor"> Core Concepts </a>

### <a href="#development-container" name="development-container" class="anchor"> Development Container </a>

A **development container** is a container in which a user can develop an application. Tools that implement the specification should provide a set of features/commands that give flexibility to users and allow development containers to scale to large development groups.

### <a href="#environment" name="environment" class="anchor"> Environment </a>

An **environment** is defined as a logical instance of one or more **development containers**, along with any needed side-car containers. An environment is based on one set of metadata that can be managed as a single unit. Users can create multiple **environments** from the same configuration metadata for different purposes.

### <a href="#project-workspace-folder" name="project-workspace-folder" class="anchor"> Project Workspace Folder </a>

The **project workspace folder** is where an implementing tool should begin to search for `devcontainer.json` files. If the target project on disk is using git, the **project workspace folder** is typically the root of the git repository.

## <a href="#configuration" name="configuration" class="anchor"> Configuration </a>

### <a href="#devcontainer-json" name="devcontainer-json" class="anchor"> devcontainer.json </a>

A **devcontainer.json** file is a JSON with Comments file that contains the metadata and configuration information needed to create and configure a development container. It defines the environment, tools, settings, and other properties required for the development container.

The file can be located at:
- `.devcontainer/devcontainer.json`
- `.devcontainer.json`
- `.devcontainer/<folder>/devcontainer.json` (where `<folder>` is a sub-folder, one level deep)

### <a href="#devcontainer-feature-json" name="devcontainer-feature-json" class="anchor"> devcontainer-feature.json </a>

A **devcontainer-feature.json** file contains metadata for a Development Container Feature. This file is located in the root folder of the feature and describes the feature's properties, options, and dependencies.

## <a href="#features" name="features" class="anchor"> Features </a>

### <a href="#development-container-feature" name="development-container-feature" class="anchor"> Development Container Feature </a>

**Development Container Features** are self-contained, shareable units of installation code and development container configuration. The name comes from the idea that referencing one of them allows you to quickly and easily add more tooling, runtime, or library "features" into your development container for you or your collaborators to use.

### <a href="#feature-equality" name="feature-equality" class="anchor"> Feature Equality </a>

The specification defines two Features as equal if both Features point to the same exact contents and are executed with the same options.

**For Features published to an OCI registry**, two Features are identical if their manifest digests are equal, and the options executed against the Feature are equal (compared value by value). Identical manifest digests implies that the tgz contents of the Feature and its entire `devcontainer-feature.json` are identical.

**For Features fetched by HTTPS URI**, two Features are identical if the contents of the tgz are identical (hash to the same value), and the options executed against the Feature are equal (compared value by value).

**For local Features**, each Feature is considered unique and not equal to any other local Feature.

### <a href="#round-stable-sort" name="round-stable-sort" class="anchor"> Round Stable Sort </a>

To prevent non-deterministic behavior in feature installation, the algorithm will sort each **round** according to specific rules:

- Compare and sort each Feature lexicographically by their fully qualified resource name
- Compare and sort each Feature from oldest to newest tag (`latest` being the "most new")
- Compare and sort each Feature by their options by greatest number of user-defined options
- Sort the provided option keys and values lexicographically
- Sort Features by their canonical name

If there is no difference based on these comparator rules, the Features are considered equal.

## <a href="#orchestration" name="orchestration" class="anchor"> Orchestration </a>

### <a href="#image-based" name="image-based" class="anchor"> Image-based </a>

An **image-based** development container configuration uses a pre-built container image as the foundation. The `image` property in `devcontainer.json` specifies the container image to use.

### <a href="#dockerfile-based" name="dockerfile-based" class="anchor"> Dockerfile-based </a>

A **Dockerfile-based** development container configuration builds a custom container image using a Dockerfile. The `dockerfile` property in `devcontainer.json` specifies the path to the Dockerfile to use.

### <a href="#docker-compose-based" name="docker-compose-based" class="anchor"> Docker Compose-based </a>

A **Docker Compose-based** development container configuration uses Docker Compose to orchestrate multiple containers. The `dockerComposeFile` property in `devcontainer.json` specifies the Docker Compose file(s) to use.

## <a href="#lifecycle" name="lifecycle" class="anchor"> Lifecycle </a>

### <a href="#lifecycle-scripts" name="lifecycle-scripts" class="anchor"> Lifecycle Scripts </a>

**Lifecycle scripts** are commands that run at specific points in the development container lifecycle:

- **onCreateCommand**: Runs when the container is created for the first time
- **postCreateCommand**: Runs after the container is created and started
- **postStartCommand**: Runs every time the container starts
- **postAttachCommand**: Runs when tools attach to the container

### <a href="#customizations" name="customizations" class="anchor"> Customizations </a>

**Customizations** are tool-specific configurations that can be applied to the development container. These are specified in the `customizations` property of `devcontainer.json` and allow tools to configure their specific settings, extensions, or other properties within the container environment.

## <a href="#metadata" name="metadata" class="anchor"> Metadata </a>

### <a href="#image-metadata" name="image-metadata" class="anchor"> Image Metadata </a>

**Image metadata** refers to the additional information that can be stored in container images to provide context about development container configuration. This metadata is added to the image as a `devcontainer.metadata` label with a JSON string value.

### <a href="#merge-logic" name="merge-logic" class="anchor"> Merge Logic </a>

**Merge logic** defines how configuration properties are combined when multiple sources provide the same property. Different properties have different merge behaviors - some use the last value, others merge arrays, and some use boolean OR logic.