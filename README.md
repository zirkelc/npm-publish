# NPM Publish Action

This is a composite action that publishes a package to NPM and creates a release on GitHub.

Steps:
- [JS-DevTools/npm-publish@v3](https://github.com/JS-DevTools/npm-publish)
- [ncipollo/release-action@v1](https://github.com/ncipollo/release-action)


## Usage

Here's an example of how to use this action in a workflow file:

```yaml
name: CI

on:
  push:
    branches:
      - main

permissions:
  id-token: write
  contents: write

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4

      - run: npm install

      - run: npm test

  release:
    name: Release
    runs-on: ubuntu-latest
    needs: [test]

    steps:
      - uses: actions/checkout@v4

      - name: actions/setup-node@v4

      - uses: zirkelc/npm-publish@v1
        with:
          token: ${{ secrets.NPM_TOKEN }}
          dry-run: false
          provenance: true
```

## Inputs

| Input         | Description                   | Required | Default |
|---------------|-------------------------------|----------|---------|
| token         | NPM token                     | true     |         |
| dry-run       | Dry run                       | false    | false   |
| provenance    | Provenance                    | false    | false   |

### Token

The `token` should be allowed to publish packages to NPM without 2FA and it should be stored in the repository secrets.

### Dry run

If `dry-run` is true, the action will not publish the package to NPM.

### Provenance

If `provenance` is true, the action will publish the package with provenance enabled.

## Permissions

| Permission | Read / Write | Description |
|------------|--------------|-------------|
| id-token   | Write        | Needed to publish to NPM with provenance |
| contents   | Write        | Needed to create a GitHub release |


