# Scenario: AI-Generated Alternatives

Use an LLM to generate multiple implementation alternatives, then let
simulation decide which one deserves production authority.

## Story

Your team asks an AI assistant to generate three implementations of a
data-processing function. Rather than code-reviewing and debating which
one to ship, you simulate all three against realistic traffic and let
KPI results make the decision.

## What this demonstrates

- AI can generate candidates; simulation can validate them.
- You don't need to trust the AI's reasoning about performance — measure it.
- The simulation is the source of truth, not the model's confidence.

## Candidates

| Candidate | Description |
|-----------|-------------|
| `baseline` | Current hand-written implementation |
| `ai-option-a` | AI-generated: simple iterative approach |
| `ai-option-b` | AI-generated: batch-optimized approach |
| `ai-option-c` | AI-generated: cache-heavy approach |

## Running

```bash
brotni simulate run \
  --simulation scenarios/ai-generated-alternatives/simulation.yaml \
  --dry-run
```

## Workflow

1. Prompt your AI assistant to generate N implementations.
2. Package each as an OCI image (or reference a Git branch).
3. Add them as candidates in this simulation spec.
4. Run the simulation.
5. Promote the winner — or ask the AI to iterate based on the KPI results.
