# Core concepts

## Simulation-first validation

Traditional software deployment relies on staging environments, canary releases,
and post-deployment monitoring. These approaches validate the code *after* it
is already running in or near production.

Simulation-first validation inverts this. Before a candidate touches production,
you:

1. Define what success looks like (KPIs, scoring strategy).
2. Capture or synthesize realistic traffic and context.
3. Run the candidate in an isolated environment against that traffic.
4. Measure and compare.
5. Decide based on evidence.

---

## Key concepts

### Simulation campaign

A **simulation campaign** is the top-level spec that ties everything together:
candidates, runtime recipe, context, replay data, scoring, and notifications.

See [`templates/simulation.yaml`](../templates/simulation.yaml).

### Candidate

A **candidate** is a version of your software you want to evaluate. Candidates
are typically OCI images but can also be Git refs or service endpoints.

Each simulation has:
- Exactly one **baseline** — the current production behavior to compare against.
- One or more **challengers** — the versions you want to evaluate.

### Runtime recipe

A **runtime recipe** defines *how* to run a candidate: which ports to expose,
environment variables, resource limits, readiness probes, and how to collect
metrics.

See [`templates/runtime.container-service.yaml`](../templates/runtime.container-service.yaml).

### Context snapshot

A **context** defines the state of the environment the candidate runs in:
configuration, downstream dependencies, feature flags, and data state.

Brotni clones the context for each candidate so they are isolated from each
other but share the same starting conditions.

See [`templates/context.snapshot.yaml`](../templates/context.snapshot.yaml).

### Replay

**Replay** is the process of running historical or synthetic events through
the candidates. The replay engine controls timing (real-time, accelerated,
or as-fast-as-possible) and routes each event to each candidate independently.

### KPI

A **KPI** (Key Performance Indicator) is a measured value used to score
candidates. Common KPIs:

| KPI | Metric | Direction |
|-----|--------|-----------|
| p99 latency | `http_request_duration_p99` | lower is better |
| error rate | `http_error_rate` | lower is better |
| cost per request | `estimated_cost_per_request` | lower is better |
| throughput | `http_requests_per_second` | higher is better |

### Scoring strategy

Brotni supports three scoring strategies:

- **compare** — compare challenger(s) against the baseline
- **rank** — rank all candidates (baseline and challengers) against each other
- **threshold** — fail if any KPI falls below an absolute threshold

### Promotion threshold

A **promotion threshold** defines how much better a challenger must be before
it is considered a candidate for production promotion. Set to `0.0` to promote
the best option regardless of margin.

---

## Flow summary

```
define simulation
       │
       ▼
define runtime recipe
       │
       ▼
define context snapshot
       │
       ▼
submit candidates (OCI images / Git refs)
       │
       ▼
replay dataset through each candidate
       │
       ▼
collect & compare KPI results
       │
       ▼
promote winner to production authority
```
