# actions-updatecli-apply
## Description
Action that runs updatecli, commits those changes if they exist, and opens a PR

## Inputs
| Input | Desription | Required |
|-------|------------|----------|
| manifest-path | Path to the updatecli manifest file. | true |
| run-type | The type of updatecli run to perform. (major or minor) | true |

## Usage
```yaml
name: updatecli-minor
"on":
  schedule:
    #       ┌───────────── minute (0 - 59)
    #       │  ┌───────────── hour (0 - 23)
    #       │  │  ┌───────────── day of the month (1 - 31)
    #       │  │  │ ┌───────────── month (1 - 12 or JAN-DEC)
    #       │  │  │ │ ┌───────────── day of the week (0 - 6 or SUN-SAT)
    #       │  │  │ │ │
    #       │  │  │ │ │
    #       │  │  │ │ │
    - cron: 0 15 * * 1  # Monday @ 3pm UTC
  workflow_dispatch:
# Do not grant jobs any permissions by default
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
        uses: prefecthq/actions-updatecli-apply
        with:
          manifest-path: '.github/updatecli/manifest-minor.yaml'
          run-type: 'minor'
```