# Recipe: container-service/minimal

The simplest runtime recipe for a containerized HTTP service.

## What it does

- Starts the candidate OCI image as a container.
- Exposes port `8080` for HTTP traffic.
- Waits for `/healthz` to return `200` before routing traffic.
- Scrapes Prometheus metrics from `/metrics`.

## Requirements for your service

Your service image must:

1. Listen on port `8080` (configurable).
2. Serve `GET /healthz` → HTTP 200 when ready.
3. Optionally expose Prometheus metrics on `GET /metrics`.

## Usage

Reference this recipe from a simulation spec:

```yaml
runtime:
  recipeRef: recipes/container-service/minimal/recipe.yaml
```

Or copy `recipe.yaml` and inline it in your simulation spec under `runtime:`.

## Customization

| Field | Default | Description |
|-------|---------|-------------|
| `container.ports[].containerPort` | `8080` | Port your service listens on |
| `readiness.httpGet.path` | `/healthz` | Readiness probe path |
| `container.resources.requests.cpu` | `250m` | CPU request |
| `container.resources.requests.memory` | `256Mi` | Memory request |
