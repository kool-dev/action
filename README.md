## About

Github Action to install [kool.dev](https://kool.dev) CLI tool.

Get `kool` to run on CI the environment just like you do locally.

For more information check [Kool.dev website](https://kool.dev) or [the CLI repository on Github](https://github.com/kool-dev/kool).

## Supported Runners

This action is only available for Linux and macOS [virtual environments](https://docs.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners#supported-virtual-environments-and-hardware-resources).

## Usage

Just use `kool-dev/action@v3` at your action YAML configuration file:

```yml
on: [push, workflow_dispatch]

jobs:
  ci:
    name: CI
    runs-on: ubuntu-latest

    steps:
      - uses: kool-dev/action@v3

      - name: Checking kool.dev Version
        run: |
          kool --version
          kool start
          # ...
```

And you will get `kool` v3.x (latest version on the v3 series).

## Shared Docker image actions

The root installer and existing `kool-dev/action@v3` usage are unchanged.
Image repositories use four separate actions:

- `kool-dev/action/image-build`: native Docker builder.
- `kool-dev/action/image-parent`: tested parent ID/layer check.
- `kool-dev/action/image-stage`: push tested IDs and upload digest metadata.
- `kool-dev/action/image-publish`: validate the complete release and publish indexes.

Pin all four to the same reviewed commit, not the installer's `v3` tag.
See [image CI](image-ci.md) for configuration, release guards and Bash/`jq` tests.
