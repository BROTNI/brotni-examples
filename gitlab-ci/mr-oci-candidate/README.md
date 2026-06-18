# GitLab CI — MR OCI Candidate

This example shows how to run a Brotni simulation for every merge request in GitLab CI.

## What it does

1. Builds the Docker image for the MR branch.
2. Submits it as a challenger candidate to a Brotni simulation.
3. Posts the results as a GitLab MR note.
4. Optionally blocks the MR if the candidate does not meet the threshold.

## Files

| File | Description |
|------|-------------|
| `simulate-mr.yml` | GitLab CI include template — reference from your `.gitlab-ci.yml` |
| `simulation.yaml` | Simulation spec |

## Usage

In your `.gitlab-ci.yml`:

```yaml
include:
  - local: 'gitlab-ci/mr-oci-candidate/simulate-mr.yml'
```

Or copy `simulate-mr.yml` directly to the root of your repository.

## Required CI/CD variables

Set these in **Settings > CI/CD > Variables**:

| Variable | Description |
|----------|-------------|
| `BROTNI_API_TOKEN` | Your Brotni API token. Get one at https://app.brotni.dev |

## Dry-run mode

```bash
brotni simulate run --simulation simulation.yaml --dry-run
```
