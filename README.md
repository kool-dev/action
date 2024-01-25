## About

Github Action to install [kool.dev](https://kool.dev) CLI.

For more information check https://kool.dev or https://github.com/kool-dev/kool.

## Supported Runners

This action is only available for Linux and macOS [virtual environments](https://docs.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners#supported-virtual-environments-and-hardware-resources).

## Usage

Just use `kool-dev/action@v1` at your action YAML configuration file:

```yml
on: [push, workflow_dispatch]

jobs:
  ci:
    name: CI
    runs-on: ubuntu-latest

    steps:
      - uses: kool-dev/action@v1

      - name: Checking kool.dev Version
        run: kool --version
```
