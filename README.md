# actions-updatecli-apply
## Description
Action that runs updatecli, commits those changes if they exist, and opens a PR

## Inputs
| Input | Description | Required |
|-------|------------|----------|
| manifest-path | Path to the updatecli manifest file. | true |
| run-helm-docs | Run helm-docs | false |
| run-type | The type of updatecli run to perform. (major or minor) | true |

## Usage
```yaml
name: updatecli-minor
"on":
  schedule:
    - cron: 0 15 * * 1  # Monday @ 3pm UTC
  workflow_dispatch:
permissions: {}
jobs:
  updatecli:
    runs-on: ubuntu-latest
    permissions:
      # required to write to the repo
      contents: write
      # required to open a pr with updatecli changes
      pull-requests: write
    steps:
      - name: checkout
        uses: actions/checkout@v4
      - name: updatecli-apply
        uses: prefecthq/actions-updatecli-apply@main
        with:
          manifest-path: '.github/updatecli/manifest-minor.yaml'
          run-type: 'minor'
```