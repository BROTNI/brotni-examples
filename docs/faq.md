# FAQ

## Do I need Brotni infrastructure to use these examples?

No. All examples support a `--dry-run` mode that validates specs and returns
synthetic results without any infrastructure. You only need a `BROTNI_API_TOKEN`
when submitting real simulations.

## Can I use these examples with my own service?

Yes. The templates and CI examples are designed to be copied and adapted.
Replace the image references, port numbers, and KPI names with your own.

## What image registry should I use?

Any OCI-compatible registry works: GHCR, Docker Hub, ECR, GCR, Artifact Registry.
The examples use GHCR (`ghcr.io`) because it integrates directly with GitHub Actions.

## Can I compare more than two candidates?

Yes. Add multiple challenger entries to the `candidates:` list. Use
`scoring.strategy: rank` to rank all candidates against each other.

See [`scenarios/ai-generated-alternatives/`](../scenarios/ai-generated-alternatives/)
for an example with four candidates.

## What happens if a candidate crashes during simulation?

Brotni records the crash as a metric (`container_restart_count`). You can add
this as a KPI with `direction: lower_is_better` and a high weight to ensure
crashing candidates are ranked last.

See [`scenarios/incident-replay/`](../scenarios/incident-replay/) for an example.

## Can I use real production traffic instead of synthetic datasets?

Yes. Change `replay.source.type` from `dataset` to a live capture spec.
This requires Brotni infrastructure access and appropriate data governance
approvals. The examples use synthetic datasets to avoid any customer data exposure.

## How do I add custom KPIs beyond the built-in HTTP metrics?

Expose custom Prometheus metrics from your service on `/metrics`. Then reference
the metric name in your `scoring.kpis` configuration.

## Can I run simulations locally without the Brotni platform?

The `--dry-run` flag validates specs and returns synthetic results locally.
Full simulations require the Brotni platform for container orchestration,
traffic routing, and metric collection.

## Where do I report issues with these examples?

Open an issue at https://github.com/BROTNI/brotni-examples/issues.
