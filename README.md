# Dev Container Build and Run (devcontainers/ci)

The Dev Container Build and Run GitHub Action is aimed at making it easier to re-use [Dev Containers](https://containers.dev) in a GitHub workflow. The Action supports using a Dev Container to run commands for CI, testing, and more, along with pre-building Dev Container image. Dev Container image building supports [Dev Container Features](https://containers.dev/implementors/features/#devcontainer-json-properties) and automatically places Dev Container [metadata on an image](https://containers.dev/implementors/spec/#image-metadata) label for simplified use.

A similar [Azure DevOps Task](./docs/azure-devops-task.md) is also available!

Note that this project builds on top of [@devcontainers/cli](https://www.npmjs.com/package/@devcontainers/cli) which can be used in other automation systems.

## GitHub Action
The examples below shows usage of the GitHub Action - see the [GitHub Action documentation](./docs/github-action.md) for more details.

> **NOTE:** This Action is not currently capable of taking advantage of pre-built Codespaces. However, pre-built images are supported.


**Pre-building an image:**

```yaml
- name: Pre-build dev container image
  uses: devcontainers/ci@v0.2
  with:
    imageName: ghcr.io/example/example-devcontainer
    cacheFrom: ghcr.io/example/example-devcontainer
    push: true
```

**Using a dev container for a CI build:**

```yaml
- name: Run make ci-build in dev container
  uses: devcontainers/ci@v0.2
  with:    
    # [Optional] Even if you're not directly referencing a pre-built image,
    # you can still use the image as a cache for your Dockerfile build.
    cacheFrom: ghcr.io/example/example-devcontainer

    push: never
    runCmd: make ci-build
```

**Both at once:**

```yaml
- name: Pre-build image and run make ci-build in dev container
  uses: devcontainers/ci@v0.2
  with:
    imageName: ghcr.io/example/example-devcontainer
    cacheFrom: ghcr.io/example/example-devcontainer
    push: true
    runCmd: make ci-build
```

## CHANGELOG

### Version 0.2.0

This version updates the implementation to use [@devcontainers/cli](https://www.npmjs.com/package/@devcontainers/cli).

This brings many benefits around compatibility with Dev Containers. One key area is that [container-features](https://code.visualstudio.com/docs/remote/containers#_dev-container-features-preview) can now be used in CI.

In theory, docker-compose based Dev Containers should also work however these are currently untested and image caching is blocked on [this issue](https://github.com/devcontainers/cli/issues/10)

### Version 0.1.x

0.1.x versions were the initial version of the action/task and attempted to mimic the behaviour of Dev Containers with manual `docker` commands
