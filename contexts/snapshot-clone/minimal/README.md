# Context: snapshot-clone/minimal

The simplest context definition — uses synthetic stubs for all dependencies.
No real infrastructure or production access required.

## What it provides

- Configuration values injected as environment variables.
- HTTP stub for a downstream API with realistic latency.
- Full network isolation — candidates cannot reach real external services.

## Usage

Reference this context from a simulation spec:

```yaml
context:
  contextRef: contexts/snapshot-clone/minimal/context.yaml
```

## Upgrading to a real snapshot

To use a real production snapshot instead of synthetic stubs, change `source.type`:

```yaml
spec:
  source:
    type: snapshot-clone
    environment: production
    region: us-east-1
```

This requires Brotni infrastructure access and appropriate permissions.
