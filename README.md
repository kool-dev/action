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
