# Scenario: Incident Replay

Replay a captured incident to verify that a fix actually resolves the issue
before deploying to production.

## Story

On January 15, 2024, a memory leak caused the service to OOM-crash under
sustained load. A fix was developed. This scenario replays the incident traffic
against the fixed candidate to confirm the fix holds.

## What makes this pattern powerful

- You know exactly what traffic triggered the incident.
- You can replay it deterministically, as many times as needed.
- The fix candidate must survive the same conditions that killed the baseline.

## Candidates

| Candidate | Description |
|-----------|-------------|
| `baseline` | The version that was running during the incident |
| `fix-candidate` | The patched version with the memory leak fixed |

## Running

```bash
# Dry-run with synthetic incident dataset
brotni simulate run \
  --simulation scenarios/incident-replay/simulation.yaml \
  --dry-run

# Full run with the real captured incident dataset (if available)
brotni simulate run \
  --simulation scenarios/incident-replay/simulation.yaml \
  --dataset path/to/real-incident-capture.jsonl
```

## Dataset

Uses `datasets/sample-events/incident-traffic.jsonl` — a synthetic dataset
modeled on a sustained-load pattern with 30-minute burst that triggers the
memory leak. No real customer data included.
