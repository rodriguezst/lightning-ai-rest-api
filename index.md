---
layout: default
title: Lightning AI REST API Reference
nav_order: 1
---

# Lightning AI REST API Reference

This is an **unofficial** REST API reference for [Lightning AI Studios](https://lightning.ai), reverse-engineered from the [`lightning-sdk`](https://pypi.org/project/lightning-sdk/) Python package (v2026.2.6+). Lightning AI does not publish an official REST API specification.

> **Disclaimer:** This documentation is community-maintained and derived from the internal OpenAPI spec embedded in the `lightning-sdk` package. Endpoints may change without notice. For production use, prefer the official [`lightning-sdk`](https://pypi.org/project/lightning-sdk/) Python package.

---

## Overview

The Lightning AI REST API allows you to programmatically manage all resources on the Lightning AI platform:

| Resource | Description |
|----------|-------------|
| [Studios](docs/studios.md) | Create, start, stop, delete, and run commands in Studios (cloud development environments) |
| [Jobs](docs/jobs.md) | Submit and manage async compute jobs |
| [Storage](docs/storage.md) | Upload and download files, manage artifacts |
| [Authentication](docs/authentication.md) | Authenticate with API keys or JWT tokens |
| [Complete Reference](docs/complete-reference.md) | All 638 endpoints across all 48 service groups |

---

## Base URL

All API requests are made to:

```
https://lightning.ai
```

All endpoints are prefixed with `/v1/`.

---

## Quick Start

### 1. Get your credentials

Log in to [lightning.ai](https://lightning.ai), go to **Settings → API Keys**, and create an API key. Note your `User ID` and `API Key`.

### 2. Set environment variables

```bash
export LIGHTNING_USER_ID="your-user-id"
export LIGHTNING_API_KEY="your-api-key"
```

### 3. Make your first request

List your teamspaces (projects):

```bash
USER_ID="your-user-id"
API_KEY="your-api-key"
AUTH=$(echo -n "${USER_ID}:${API_KEY}" | base64)

curl -s "https://lightning.ai/v1/projects" \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" | jq .
```

---

## Authentication

The API supports two authentication schemes:

### API Key (Basic Auth) — Recommended

Encode `userId:apiKey` in Base64 and pass as a `Basic` Authorization header:

```bash
AUTH=$(echo -n "USER_ID:API_KEY" | base64)
curl -H "Authorization: Basic ${AUTH}" https://lightning.ai/v1/...
```

### JWT Bearer Token

Exchange credentials for a short-lived JWT:

```bash
curl -X POST https://lightning.ai/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "your-email@example.com", "apiKey": "your-api-key"}'
```

Use the returned token as:

```bash
curl -H "Authorization: Bearer YOUR_JWT_TOKEN" https://lightning.ai/v1/...
```

See the full [Authentication Guide](docs/authentication.md) for details.

---

## Key Concepts

### Projects (Teamspaces)

In the REST API, teamspaces are called **projects**. Every resource (Studio, Job, Storage) belongs to a project, identified by `projectId`.

```bash
# List all your projects
curl -H "Authorization: Basic ${AUTH}" https://lightning.ai/v1/projects
```

### Studios (CloudSpaces)

Studios are interactive cloud development environments. In the API, they are called **cloudspaces**.

```bash
# List studios in a project
curl -H "Authorization: Basic ${AUTH}" \
  https://lightning.ai/v1/projects/{projectId}/cloudspaces
```

### Machine Types (Compute)

When starting or switching a Studio, specify a machine by its **slug**:

| Machine | Slug | vCPUs / GPUs |
|---------|------|--------------|
| CPU (default) | `cpu-4` | 4 vCPUs |
| CPU Small | `cpu-2` | 2 vCPUs |
| CPU Large | `cpu-8` | 8 vCPUs |
| T4 | `lit-t4-1` | 1× NVIDIA T4 |
| T4 × 2 | `lit-t4-2` | 2× NVIDIA T4 |
| L4 | `lit-l4-1` | 1× NVIDIA L4 |
| A100 | `lit-a100-1` | 1× NVIDIA A100 |
| A100 × 8 | `lit-a100-8` | 8× NVIDIA A100 |
| H100 | `lit-h100-1` | 1× NVIDIA H100 |
| H100 × 8 | `lit-h100-8` | 8× NVIDIA H100 |
| H200 | `lit-h200x-1` | 1× NVIDIA H200 |
| B200 × 8 | `lit-b200x-8` | 8× NVIDIA B200 |

---

## Response Format

All responses are JSON. Successful responses return the requested resource or a response object. Errors follow this format:

```json
{
  "code": 5,
  "message": "studio 'my-studio' does not exist",
  "details": []
}
```

### Common HTTP Status Codes

| Code | Meaning |
|------|---------|
| `200` | Success |
| `400` | Bad Request — invalid parameters |
| `401` | Unauthorized — invalid or missing credentials |
| `404` | Not Found |
| `409` | Conflict — resource already exists |
| `429` | Too Many Requests — rate limited |
| `500` | Internal Server Error |
| `503` | Service Unavailable |

---

## Rate Limiting & Retries

The `lightning-sdk` implements exponential backoff with the following retry policy:

- Retries on HTTP errors and 5xx responses
- Retries on 4xx errors except 400, 401, and 404
- Default backoff: up to 5 minutes max
- Default max retries: 7 (configurable)

---

## Source

This documentation is generated from `lightning-sdk` version `2026.2.6`. The SDK source is available on [PyPI](https://pypi.org/project/lightning-sdk/).

- [studios.md](docs/studios.md) — Studios (CloudSpaces) API
- [jobs.md](docs/jobs.md) — Jobs & Deployments API
- [storage.md](docs/storage.md) — Storage & File Transfer API
- [authentication.md](docs/authentication.md) — Authentication API
- [complete-reference.md](docs/complete-reference.md) — Complete Endpoint Reference
