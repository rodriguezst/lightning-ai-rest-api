# Lightning AI REST API Reference

Unofficial REST API documentation for [Lightning AI Studios](https://lightning.ai), reverse-engineered from the [`lightning-sdk`](https://pypi.org/project/lightning-sdk/) Python package (v2026.2.6).

**Published documentation:** [https://rodriguezst.github.io/lightning-ai-rest-api](https://rodriguezst.github.io/lightning-ai-rest-api)

---

## What is this?

Lightning AI does not publish an official REST API specification. This project documents the REST API by analyzing the `lightning-sdk` Python package's internal OpenAPI client code.

The documentation covers **638 endpoints** across **48 service groups**.

---

## Documentation

| Page | Description |
|------|-------------|
| [Home / Overview](index.md) | Base URL, quick start, key concepts, machine types |
| [Authentication](docs/authentication.md) | API keys, JWT tokens, environment variables |
| [Studios](docs/studios.md) | Create, start, stop, run commands, plugins, ports |
| [Jobs & Deployments](docs/jobs.md) | Async jobs, multi-machine training, inference deployments |
| [Storage & Files](docs/storage.md) | File upload/download, datasets, models, secrets |
| [Complete Reference](docs/complete-reference.md) | All 638 REST endpoints |

---

## Quick Start

```bash
# Set your credentials
export LIGHTNING_USER_ID="your-user-id"
export LIGHTNING_API_KEY="your-api-key"

# Build auth header
AUTH=$(echo -n "${LIGHTNING_USER_ID}:${LIGHTNING_API_KEY}" | base64)

# List your teamspaces (projects)
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects" | jq '.projects[].name'

# List studios in a project
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces" | jq '.cloudspaces[].name'

# Start a studio
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/start" \
  -d '{"compute_config": {"name": "cpu-4", "spot": false}}'

# Run a command in a studio
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/execute" \
  -d '{"command": "python --version"}'
```

---

## Source

Extracted from `lightning-sdk` v2026.2.6 (PyPI). This documentation is provided as-is and may become outdated as Lightning AI evolves its internal API.

---

## Disclaimer

This is an **unofficial** community resource. Use the official [`lightning-sdk`](https://pypi.org/project/lightning-sdk/) Python package for production use cases, as it handles authentication, retries, and API compatibility automatically.
