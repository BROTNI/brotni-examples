# Scenario: Routing Optimization

An end-to-end scenario comparing two routing algorithm implementations:
a baseline nearest-node algorithm versus a challenger latency-aware algorithm.

## Story

Your team has a routing service that assigns incoming requests to backend nodes.
A new latency-aware algorithm claims to reduce p99 latency by routing away from
hot nodes. This scenario lets you verify that claim before production.

## What's included

| File | Description |
|------|-------------|
| `simulation.yaml` | Full simulation spec for this scenario |
| `candidates/baseline/` | Baseline routing service (nearest-node) |
| `candidates/challenger/` | Challenger routing service (latency-aware) |

## Running the scenario

### Dry-run (no infrastructure needed)

```bash
brotni simulate run \
  --simulation scenarios/routing-optimization/simulation.yaml \
  --dry-run
```

### Full simulation

```bash
brotni simulate run \
  --simulation scenarios/routing-optimization/simulation.yaml
```

## Expected outcome

The challenger's latency-aware algorithm should show:

- Lower p99 latency by routing away from overloaded nodes.
- Similar or lower error rate.
- Slightly higher CPU usage (acceptable trade-off).

## Dataset

Uses `datasets/sample-events/routing-requests.jsonl` — a synthetic workload
of 10,000 routing requests with realistic burst patterns.
