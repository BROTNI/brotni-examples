# Scenario: Cost vs Latency

A scenario that explores the trade-off between infrastructure cost and
request latency across different service configurations.

## Story

Your team is evaluating whether to scale down your service from 4 replicas
to 2 replicas to reduce cloud costs. The question: does the cost saving
come at an unacceptable latency penalty under realistic traffic?

## Candidates

| Candidate | Configuration | Expected behavior |
|-----------|---------------|-------------------|
| `baseline` | 4 replicas, standard nodes | Lower latency, higher cost |
| `challenger` | 2 replicas, standard nodes | Lower cost, potentially higher latency |

## What you learn

- The p99 latency impact of halving the replica count.
- Whether the error rate stays within acceptable limits.
- The cost saving in estimated `$/request`.

## Running

```bash
# Dry-run (no infrastructure)
brotni simulate run \
  --simulation scenarios/cost-vs-latency/simulation.yaml \
  --dry-run

# Full run
brotni simulate run \
  --simulation scenarios/cost-vs-latency/simulation.yaml
```
