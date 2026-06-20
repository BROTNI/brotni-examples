# brotni-examples

Brotni Examples helps teams simulate, score, and compare software candidates before production.

This repository contains public examples, templates, CI/CD recipes, context definitions, and demo scenarios for [Brotni](https://brotni.dev)-style simulation-first validation.

---

## What is simulation-first validation?

Instead of deploying a candidate to production and hoping for the best, you:

1. **Define a simulation** — describe the workload, scoring criteria, and comparison strategy.
2. **Define a runtime recipe** — specify how the candidate runs (container, service, etc.).
3. **Define a context** — capture a snapshot of production state (traffic, config, dependencies).
4. **Submit one or more candidates** — OCI images, Git refs, or service endpoints.
5. **Replay data** — run historical or synthetic events through the candidates.
6. **Compare KPI results** — latency, cost, error rate, business metrics.
7. **Choose what deserves production authority** — promote only what actually performs better.

---

## Repository structure

```
brotni-examples/
├── templates/                  # Reusable YAML templates
│   ├── simulation.yaml
│   ├── runtime.container-service.yaml
│   └── context.snapshot.yaml
├── github-actions/             # GitHub Actions workflow examples
│   ├── pr-oci-candidate/
│   └── multi-pr-campaign/
├── gitlab-ci/                  # GitLab CI pipeline examples
│   ├── mr-oci-candidate/
│   └── simulation-status/
├── recipes/                    # Runtime recipes for common service types
│   └── container-service/
│       └── minimal/
├── contexts/                   # Context snapshot definitions
│   └── snapshot-clone/
│       └── minimal/
├── scenarios/                  # End-to-end simulation scenarios
│   ├── campaign-golden-path/   # 3-candidate campaign in the implemented manifest format
│   ├── routing-optimization/
│   ├── cost-vs-latency/
│   ├── incident-replay/
│   └── ai-generated-alternatives/
├── datasets/                   # Synthetic datasets for testing
│   ├── sample-events/
│   └── sample-metrics/
└── docs/                       # Guides and reference documentation
    ├── quickstart.md
    ├── concepts.md
    └── faq.md
```

---

## Quickstart

### GitHub Actions — OCI image candidate

Add this workflow to validate every PR against a simulation:

```yaml
# .github/workflows/simulate-pr.yml
name: Simulate PR candidate

on:
  pull_request:

jobs:
  simulate:
    uses: ./.github/workflows/...
    # See github-actions/pr-oci-candidate/ for the full example
```

See [`github-actions/pr-oci-candidate/`](github-actions/pr-oci-candidate/) for the complete working example.

### GitLab CI — MR candidate

```yaml
# .gitlab-ci.yml
include:
  - local: '.gitlab-ci/simulate-mr.yml'
```

See [`gitlab-ci/mr-oci-candidate/`](gitlab-ci/mr-oci-candidate/) for the complete example.

### OCI image candidate

Any Docker-compatible image can be submitted as a candidate:

```yaml
candidates:
  - id: pr-123
    image: ghcr.io/my-org/my-service:pr-123
    role: challenger
  - id: main
    image: ghcr.io/my-org/my-service:main
    role: baseline
```

### Local CLI dry-run

```bash
# Install the Brotni CLI
curl -sSL https://get.brotni.dev | sh

# Validate your simulation spec (no infrastructure needed)
brotni simulate validate --file templates/simulation.yaml --dry-run

# Submit a local candidate for dry-run scoring
brotni simulate run \
  --simulation templates/simulation.yaml \
  --recipe recipes/container-service/minimal/recipe.yaml \
  --context contexts/snapshot-clone/minimal/context.yaml \
  --dry-run
```

### Sample simulation scenario

See [`scenarios/routing-optimization/`](scenarios/routing-optimization/) for a complete end-to-end example comparing two routing algorithm implementations.

See [`scenarios/campaign-golden-path/`](scenarios/campaign-golden-path/) for a Simulation Campaign that compares three heterogeneous candidates (two OCI images + one config bundle) using the implemented `.brotni/simulation.yaml` manifest, the `brotni-github-action`, and the `brotni-gitlab-component`.

---

## Templates

| Template | Description |
|----------|-------------|
| [`templates/simulation.yaml`](templates/simulation.yaml) | Defines the simulation campaign, scoring, and comparison strategy |
| [`templates/runtime.container-service.yaml`](templates/runtime.container-service.yaml) | Specifies how to run a containerized service candidate |
| [`templates/context.snapshot.yaml`](templates/context.snapshot.yaml) | Captures a context snapshot for replay |

---

## Example philosophy

All examples in this repository are:

- **Small** — easy to read in one sitting
- **Readable** — clear naming, minimal boilerplate
- **Realistic** — based on common production patterns
- **Copyable** — designed to be adapted directly
- **Safe** — no secrets, no real customer data
- **Independent** — work with dry-run mode where real infrastructure is not available
- **Educational** — each example teaches a concept

---

## Contributing

Examples are most useful when they match real-world patterns. If you have a scenario that helped your team, consider contributing it.

See [`docs/contributing.md`](docs/contributing.md) for guidelines.

---

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
