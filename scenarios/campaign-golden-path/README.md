# Scenario: Campaign golden path

An end-to-end **Simulation Campaign** comparing **three heterogeneous candidates**
under one work item, using the manifest, recipes, action, and component that
Brotni actually implements.

> This scenario uses the implemented `.brotni/simulation.yaml` campaign schema
> (`version: 1`, validated by `brotni validate campaign`) and the
> `recipe.brotni.com/v1` recipe schemas — not the illustrative `brotni.dev/v1`
> format used by some other examples in this repo.

## The candidates

| Candidate | Source kind | Identity |
|-----------|-------------|----------|
| `routing-policy-a` | `container_image` | OCI digest (PR #501) |
| `routing-policy-b` | `container_image` | OCI digest (PR #502) |
| `routing-config-v7` | `config_bundle` | config hash |

All three are compared on the **same goals** (latency, cost, resilience) and the
**same blocking constraints** (p99 latency, error rate).

## Files

| File | Description |
|------|-------------|
| [`.brotni/simulation.yaml`](.brotni/simulation.yaml) | The campaign manifest: work item, goals, constraints, candidates |
| [`runtime.container-service.yaml`](runtime.container-service.yaml) | Recipe for the two OCI image candidates |
| [`config-bundle.yaml`](config-bundle.yaml) | Recipe for the configuration-only candidate |
| [`github-actions/campaign.yml`](github-actions/campaign.yml) | GitHub Actions workflow using `BROTNI/brotni-github-action` |
| [`gitlab-ci/campaign.gitlab-ci.yml`](gitlab-ci/campaign.gitlab-ci.yml) | GitLab pipeline using `brotni-gitlab-component` |

## Walkthrough

1. **Open a work item** — e.g. GitHub issue #482, "Improve routing under high traffic".
2. **Create candidates** — two PRs with alternative routing policies and one
   configuration-only change. Label the PRs `brotni-simulation-candidate`.
3. **Validate the manifest** locally:
   ```bash
   brotni validate campaign scenarios/campaign-golden-path/.brotni/simulation.yaml
   ```
4. **Create the campaign** — this also registers the manifest's explicit
   candidates and prints each one's studio-minted candidate ID:
   ```bash
   export BROTNI_API_URL=http://localhost:8081   # a running studio/engine
   brotni campaign create --manifest scenarios/campaign-golden-path/.brotni/simulation.yaml
   ```
   Dynamic discovery (labelled PRs/MRs) is handled by the CI integrations;
   `brotni campaign discover` surfaces that mode but does not register here.
5. **Run & collect** — each candidate runs against the same `snapshot-clone`
   context and produces metrics. For a local walkthrough you can hand-feed
   metrics per candidate (demo affordance):
   ```bash
   brotni campaign ingest --id <campaign-id> --candidate <candidate-id> \
     --metrics p99_latency_ms=120,cost_per_1k_requests=8,resilience_score=70,error_rate=0.1
   ```
6. **Compare**:
   ```bash
   brotni campaign compare --id <campaign-id>
   ```
7. **Decide**:
   ```bash
   brotni campaign decision --id <campaign-id> --format md
   ```
   Re-weighting goals creates a new scoring version — pass `--scoring-version N`
   to see how the winner changes without losing the original interpretation.
8. **Status back** — the action publishes a campaign-scoped check + PR comment
   ("Candidate N of M" + a comparison deep link); the component posts MR status
   and exposes `BROTNI_CAMPAIGN_URL`.

## Related

- Campaign model & API: `brotni-simulation-studio/docs/architecture/simulation-campaigns.md`
- Context lifecycle: [`contexts/snapshot-clone/`](../../contexts/snapshot-clone/)
- Recipes: `brotni-recipes/recipes/{container-service,config-bundle}/v1`
