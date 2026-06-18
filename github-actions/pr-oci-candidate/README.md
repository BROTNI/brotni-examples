# GitHub Actions — PR OCI Candidate

This example shows how to build and simulate an OCI image candidate on every pull request.

## What it does

1. Builds the Docker image for the PR branch.
2. Pushes it to the container registry.
3. Submits the image as a challenger candidate to a Brotni simulation.
4. Posts the simulation results as a PR comment.
5. Optionally fails the job if the challenger does not beat the baseline.

## Files

| File | Description |
|------|-------------|
| `workflow.yml` | GitHub Actions workflow — copy to `.github/workflows/` |
| `simulation.yaml` | Simulation spec for this workflow |

## Usage

1. Copy `workflow.yml` to `.github/workflows/simulate-pr.yml` in your repository.
2. Copy `simulation.yaml` to `.brotni/simulation.yaml` (or adjust the path in the workflow).
3. Add your `BROTNI_API_TOKEN` as a repository secret.
4. Open a pull request — the simulation runs automatically.

## Required secrets

| Secret | Description |
|--------|-------------|
| `BROTNI_API_TOKEN` | Your Brotni API token. Get one at https://app.brotni.dev |
| `GHCR_TOKEN` | GitHub token with `packages:write` scope (usually `GITHUB_TOKEN` works) |

## Dry-run mode

You can run the simulation without real infrastructure:

```bash
brotni simulate run --simulation simulation.yaml --dry-run
```
