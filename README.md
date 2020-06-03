# tfupdate-action

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Release](https://img.shields.io/github/v/release/sharu1204/tfupdate-action)](https://github.com/sharu1204/tfupdate-action/releases)

## About

Integrating [tfupdate](https://github.com/minamijoyo/tfupdate) with Github Action to update

- Terraform
- Terraform Providers
- Terraform Modules

to the latest version with v0.12+ support (`tdupdate` originally supports it).

## Behavior

When `tfupdate-action` is triggered, it checks the latest version of the resource you specified. If there is an update, it creates a pull request to bump a version against the base branch you specified.

If you want to skip the current latest version for some reason, you can simply close the pull request. `tfupdate-action` will not create another pull reuqest for the same version.

## Inputs

### `resource`

**Required** The name of resource to be updated. Either `"terraform"` `"provider" or "module"`.

### `recursive`

**Optional** Whether to check a directory recursively or not. Default to `"false"`.

### `ignore_path`

**Optional** A regular expression for path to be ignored.

### `file_path`

**Optional** A path to file or directory to be updated. Default to `"./"`

### `provider`

**Optional** The name of provider. *Required* when setting `"provider"` resource.

### `module_name`

**Optional** The name of module. *Required* when setting `"module"` resource.

### `source_type`

**Optional** The source type of module. *Required* when setting `"module"` resource. Either `"github"`, `"gitlab"` or `"tfregistryModule"`.

### `base_branch`

**Optional** The branch name against which a PR will be created. Default to `"master"`.

### `label`

**Optional** The label name added to a PR. Default to `"dependencies"`. If the lable does not exit, it will be automatically created.

### `reviewer`

**Optional** A reviewer to be assigned to a PR. You can assign multiple reviewers with a comma-separated list (no spaces around the comma).

## Example Usage

```yml
name: Terraform Update

on:
  schedule:
    - cron: "0 11 * * 1-5"

env:
  # To use hub command in Github Action: https://github.com/github/hub#github-actions
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  # If you use personal token, replace email/user accordingly
  USER_EMAIL: action@github.com
  USER_NAME: GitHub Action

jobs:
  run_tfupdate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Update terraform
        uses: sharu1204/tfupdate-action@v0.1.1
        with:
          resource: "terraform"
          file_path: "./tf/config.tf"

      - name: Update terraform provider
        uses: sharu1204/tfupdate-action@v0.1.1
        with:
          resource: "provider"
          provider_name: "aws"
          file_path: "./tf/config.tf"

      - name: Update terraform module
        uses: sharu1204/tfupdate-action@v0.1.1
        with:
          resource: "module"
          module_name: "terraform-aws-modules/vpc/aws"
          source_type: "tfregistryModule"
          file_path: "./tf/main.tf"
```

## Acknowledgement

As `tfupdate-action` Github Action is a wrapper of `tfpudate` command, it much owes to https://github.com/minamijoyo/tfupdate implementation. For details of inputs parameters, please refer to its original usage guide.
