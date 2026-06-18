# GitHub Actions — Multi-PR Campaign

This example shows how to run a simulation campaign that compares multiple
pull request candidates against each other and a baseline simultaneously.

## What it does

1. Collects a list of open PRs labeled `simulate`.
2. Builds an OCI image for each PR.
3. Submits all images as candidates in a single simulation campaign.
4. Posts a ranked comparison to each PR.

## When to use this

Use a multi-PR campaign when you want to:
- Compare several feature branches in one shot.
- Run periodic (nightly) ranking of all in-flight candidates.
- Decide which of several competing implementations to merge first.

## Files

| File | Description |
|------|-------------|
| `workflow.yml` | GitHub Actions workflow — copy to `.github/workflows/` |
| `campaign.yaml` | Multi-candidate campaign spec |

## Usage

1. Copy `workflow.yml` to `.github/workflows/simulate-campaign.yml`.
2. Label the PRs you want to include with `simulate`.
3. The workflow triggers nightly or when manually dispatched.

## Required secrets

| Secret | Description |
|--------|-------------|
| `BROTNI_API_TOKEN` | Your Brotni API token |
