# Quickstart

Get from zero to a working simulation in under 10 minutes.

## Prerequisites

- Docker (for building OCI images)
- The Brotni CLI:
  ```bash
  curl -sSL https://get.brotni.dev/install.sh | sh
  ```
- A `BROTNI_API_TOKEN` (get one at https://app.brotni.dev) — not needed for dry-runs

---

## Step 1 — Validate a template (no infrastructure needed)

Run a dry-run validation to confirm the simulation spec is well-formed:

```bash
brotni simulate validate \
  --file templates/simulation.yaml \
  --dry-run
```

Expected output:
```
✓ Spec is valid
✓ 2 candidates defined (1 baseline, 1 challenger)
✓ 3 KPIs configured
✓ Dry-run mode: no infrastructure required
```

---

## Step 2 — Run an example scenario

```bash
brotni simulate run \
  --simulation scenarios/routing-optimization/simulation.yaml \
  --dry-run
```

The dry-run mode:
- Validates all specs.
- Uses the sample dataset from `datasets/sample-events/routing-requests.jsonl`.
- Returns synthetic scores (not real measurements).
- Does not require Brotni infrastructure or an API token.

---

## Step 3 — Wire it to your CI pipeline

### GitHub Actions

Copy the workflow template to your repository:

```bash
cp github-actions/pr-oci-candidate/workflow.yml \
   your-repo/.github/workflows/simulate-pr.yml

cp github-actions/pr-oci-candidate/simulation.yaml \
   your-repo/.brotni/simulation.yaml
```

Then add `BROTNI_API_TOKEN` as a repository secret in **Settings > Secrets**.

### GitLab CI

Add to your `.gitlab-ci.yml`:

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/BROTNI/brotni-examples/main/gitlab-ci/mr-oci-candidate/simulate-mr.yml'
```

Set `BROTNI_API_TOKEN` in **Settings > CI/CD > Variables**.

---

## Step 4 — Submit your first real candidate

```bash
# Build your service image
docker build -t my-service:pr-1 .

# Push to a registry
docker push ghcr.io/my-org/my-service:pr-1

# Run a simulation against the main branch baseline
brotni simulate run \
  --simulation .brotni/simulation.yaml \
  --candidate id=challenger,image=ghcr.io/my-org/my-service:pr-1
```

---

## Next steps

- Read [concepts.md](concepts.md) for an explanation of simulations, recipes, and contexts.
- Explore the [scenarios/](../scenarios/) directory for end-to-end examples.
- Check [faq.md](faq.md) for common questions.
