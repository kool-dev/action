## About

Github Action to install [kool.dev](https://kool.dev) CLI.

For more information check https://kool.dev or https://github.com/kool-dev/kool.

## Supported Runners

This action is only available for Linux and macOS [virtual environments](https://docs.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners#supported-virtual-environments-and-hardware-resources).

## Usage

Just uses `kool-dev/action@master` at your action yml file. e.g:

```yml
on: [push, workflow_dispatch]

jobs:
  ci:
    name: CI
    runs-on: ubuntu-latest

    steps:
      - uses: kool-dev/action@master

      - name: Checking kool.dev version
        run: kool --version
```
