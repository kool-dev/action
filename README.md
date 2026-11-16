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

The root action remains the Kool CLI installer. Its existing `v3` tags and
`kool-dev/action@v3` usage are unchanged.

Image repositories can use these separate actions, pinned to a reviewed commit:

```yaml
- uses: kool-dev/action/image-build@<40-character-commit>
  with:
    arch: amd64

# After image-specific builds/tests/scans:
- uses: kool-dev/action/image-stage@<same-commit>
  with:
    canonical-repository: kool-dev/docker-example
    default-branch: master
    repository: kooldev/example
    arch: amd64
    group: example
    images: '[{"tag":"example","id":"sha256:<tested-image-id>"}]'
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

`image-parent` checks fresh same-job parent IDs/layers. The reusable publisher
is `kool-dev/action/.github/workflows/image-publish.yml@<same-commit>`.
Use the same commit for every action, reusable workflow and `tooling-ref` input;
the installer's `v3` tag is not a version alias for the image tooling.

See [image CI](image-ci.md) for the pipeline, publication guards, limitations and
Bash/`jq` tests. No Python release helpers or resume workflow are required.
