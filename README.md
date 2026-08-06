# ci-templates

Shared reusable GitHub Actions workflows for this org's self-hosted runners.

## Runner setup

Each org has 2 self-hosted runners, org-level, default runner group (all repos):

- `primary` label — runs on chris-pc (local dev machine)
- `fallback` label — runs on the odroid (always-on homelab host)

## Usage: failover runner selection

Add this to any workflow to automatically use `primary` if it's online, else `fallback`:

```yaml
jobs:
  pick-runner:
    uses: trayhem-projects/ci-templates/.github/workflows/pick-runner.yml@main
    with:
      org: trayhem-projects
    secrets: inherit

  build:
    needs: pick-runner
    runs-on: ${{ fromJSON(needs.pick-runner.outputs.labels_json) }}
    steps:
      - run: echo hello from ${{ runs-on }}
```

Replace `trayhem-projects` with `bytesphere-org` or `trayhem-projects`.

## New repos

Copy the `pick-runner` job above into new workflows so they get failover
automatically. There's no GitHub Enterprise "required workflow" feature on
this plan, so this isn't enforced — just convention.
