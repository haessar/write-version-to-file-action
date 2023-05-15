# GitHub Action - Write Version To File
![Github JavaScript Actions CI/CD](https://github.com/haessar/write-version-to-file-action/workflows/Github%20JavaScript%20Actions%20CI/CD/badge.svg)

This GitHub Action writes the version information from a given variable into a file with a well known JSON structure.
Once the file is written it will perform a commit to the repository with the changed version file.

Forked from https://github.com/dolittle/write-version-to-file-action with additional functionality:
* Allow specifying additional commit author (inspired by https://github.com/stefanzweifel/git-auto-commit-action)
* Allow writing just version string to simplified text output (inspired by https://github.com/sebastian-muthwill/write-version-to-file)

## Pre requisites

node <= 16
yarn
git

## Usage

Create a workflow `.yml` file in your `.github/workflows` directory. An [example workflow](#example-workflow) is available below.

For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file)

### Inputs

- `version`: The version number to use.
- `path`: The path to the file within the repository.
- `user-email`: The email of the user that commits the version file.
- `user-name`: The name of the user that commits the version file.
- `author-email`: The email of the user that authors the version file.
- `author-name`: The name of the user that authors the version file.
- `merge-strategy`: How to reconcile divergent branches when pulling.
  - `merge`: Tries to merge (default)
  - `rebase`: Tries to rebase
  - `fast-forward`: Only allows for fast forwards
- `file-type`: The output file type containing version information.
  - `json`: version, commit SHA and build date in JSON format (default)
  - `txt`: version number only, in txt format

For information on how GitHub actions tokens work, read more [here](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/authenticating-with-the-github_token#about-the-github_token-secret).

### Example Workflow

```yaml
on:
  push:
    branches:
    - '**'
  pull_request:
    types: [closed]

name: GitHub action workflow name

jobs:
  context:
    name: Job name
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Get current version
        uses: haessar/write-version-to-file-action@v1
        with:
          version: ${{ steps.context.outputs.current-version }}
          path: ./Source/version.json
          user-email: build@dolittle.com
          user-name: dolittle-build
          merge-strategy: merge
```

## Contributing

We're always open for contributions and bug fixes!
