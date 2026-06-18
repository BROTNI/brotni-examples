# GitLab CI — Simulation Status

This example shows how to poll for and display the status of a previously submitted
Brotni simulation from a GitLab CI pipeline.

## Use case

Use this when you:
- Submitted a simulation asynchronously and want to wait for results.
- Want to gate a deployment job on simulation completion.
- Need to surface simulation status in a downstream pipeline.

## Files

| File | Description |
|------|-------------|
| `simulation-status.yml` | GitLab CI job template |

## Usage

```yaml
include:
  - local: 'gitlab-ci/simulation-status/simulation-status.yml'
```

Set `BROTNI_SIMULATION_ID` to the ID returned by a previous `brotni simulate run` call.
