---
layout: default
title: Studios (CloudSpaces)
nav_order: 3
---

# Studios API (CloudSpaces)

Studios are interactive cloud development environments in Lightning AI. In the REST API, Studios are called **cloudspaces**. Every Studio belongs to a **project** (teamspace).

**Base path:** `/v1/projects/{projectId}/cloudspaces`

---

## Prerequisites

You need a `projectId`. There is **no `GET /v1/projects` endpoint** — use `GET /v1/memberships` to discover your teamspaces:

```bash
AUTH=$(echo -n "${LIGHTNING_USER_ID}:${LIGHTNING_API_KEY}" | base64)

# Step 1: List your memberships to get teamspace (project) IDs
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/memberships?filterByUserId=true" | jq '.memberships[] | {name, projectId}'

# Step 2: Extract the first project ID (field is camelCase: projectId)
PROJECT_ID=$(curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/memberships?filterByUserId=true" | jq -r '.memberships[0].projectId')
```

---

## List Studios

**`GET /v1/projects/{projectId}/cloudspaces`**

Returns all Studios in a project.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | The project (teamspace) ID |

**Query parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | string | **Yes** | Your user ID (required — without it the response is empty) |
| `name` | string | No | Filter by Studio name (exact match) |
| `clusterId` | string | No | Filter by cloud account / cluster |
| `pageToken` | string | No | Pagination cursor |
| `limit` | integer | No | Max results per page |
| `activeOnly` | boolean | No | Return only running Studios |
| `isFavorite` | boolean | No | Return only favorited Studios |

**Response:**

```json
{
  "cloudspaces": [
    {
      "id": "01abcdef-0000-0000-0000-000000000001",
      "name": "my-studio",
      "displayName": "My Studio",
      "clusterId": "lightning-cloud",
      "codeStatus": {
        "inUse": {
          "phase": "CLOUD_SPACE_INSTANCE_STATE_RUNNING",
          "cloudSpaceInstanceId": "instance-id"
        }
      },
      "createdAt": "2024-01-15T10:00:00Z"
    }
  ]
}
```

> **Note:** All response fields use **camelCase** (e.g. `displayName`, `clusterId`, `codeStatus`, `createdAt`).

**Studio status phases:**

| Phase | SDK Status | Description |
|-------|------------|-------------|
| `null` | `Stopped` | No active instance |
| `CLOUD_SPACE_INSTANCE_STATE_UNSPECIFIED` | `Pending` | Starting |
| `CLOUD_SPACE_INSTANCE_STATE_PENDING` | `Pending` | Provisioning |
| `CLOUD_SPACE_INSTANCE_STATE_RUNNING` | `Running` | Active and ready |
| `CLOUD_SPACE_INSTANCE_STATE_STOPPING` | `Stopping` | Shutting down |
| `CLOUD_SPACE_INSTANCE_STATE_STOPPED` | `Stopped` | Inactive |
| `CLOUD_SPACE_INSTANCE_STATE_FAILED` | `Failed` | Error state |

**Example:**

```bash
# userId is required — your user ID is available from GET /v1/auth/user
USER_ID="your-user-id"

curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces?userId=${USER_ID}" | jq '.cloudspaces[].name'
```

---

## Get Studio

**`GET /v1/projects/{projectId}/cloudspaces/{id}`**

Returns a single Studio by its ID.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Example:**

```bash
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}"
```

---

## Get Studio by Name

**`GET /v1/users/{userName}/projects/{projectName}/cloudspaces/{cloudspaceName}/getbyname`**

Returns a Studio by its human-readable name. Useful when you know the names but not the IDs.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userName` | string | Yes | Username or org name |
| `projectName` | string | Yes | Project (teamspace) name |
| `cloudspaceName` | string | Yes | Studio name |

**Example:**

```bash
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/users/johndoe/projects/my-team/cloudspaces/my-studio/getbyname"
```

---

## Create Studio

**`POST /v1/projects/{projectId}/cloudspaces`**

Creates a new Studio in the specified project.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |

**Request body:**

```json
{
  "name": "my-new-studio",
  "display_name": "My New Studio",
  "cluster_id": "lightning-cloud",
  "seed_files": [
    {
      "path": "main.py",
      "contents": "print('Hello, Lightning World!')\n"
    }
  ],
  "source": null,
  "disable_secrets": false,
  "cloud_space_environment_template_id": null
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Studio identifier (URL-safe, lowercase) |
| `display_name` | string | No | Human-readable name |
| `cluster_id` | string | No | Cloud account / cluster ID (defaults to teamspace default) |
| `seed_files` | array | No | Initial files to create in the Studio |
| `source` | string | No | Source type for creating from template |
| `disable_secrets` | boolean | No | Disable automatic secrets injection |
| `cloud_space_environment_template_id` | string | No | Environment template ID (base Studio type) |

**Response:** Returns the created `V1CloudSpace` object.

**Note:** After creating a Studio, the SDK also creates a Lightning Run via:
```
POST /v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs
```

**Example:**

```bash
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces" \
  -d '{
    "name": "my-new-studio",
    "display_name": "My New Studio"
  }'
```

---

## Start Studio

**`POST /v1/projects/{projectId}/cloudspaces/{id}/start`**

Starts (powers on) a stopped Studio on the specified machine.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Request body:**

```json
{
  "compute_config": {
    "name": "cpu-4",
    "spot": false,
    "requested_run_duration_seconds": "10800"
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `compute_config.name` | string | Yes | Machine slug (e.g., `cpu-4`, `lit-t4-1`, `lit-a100-8`) |
| `compute_config.spot` | boolean | No | Use interruptible/spot instance (cheaper, but can be preempted) |
| `compute_config.requested_run_duration_seconds` | string | No | Max runtime in seconds (required for some high-end machines) |

**Machine slugs:**

| Slug | Type | GPUs/CPUs |
|------|------|-----------|
| `cpu-2` | CPU | 2 vCPUs |
| `cpu-4` | CPU | 4 vCPUs (default) |
| `cpu-8` | CPU | 8 vCPUs |
| `cpu-16` | CPU | 16 vCPUs |
| `data-prep-mid` | Data Prep | 32 vCPUs, large disk |
| `lit-t4-1` | GPU | 1× T4 |
| `lit-t4-2` | GPU | 2× T4 |
| `lit-t4-4` | GPU | 4× T4 |
| `lit-t4-8` | GPU | 8× T4 |
| `lit-l4-1` | GPU | 1× L4 |
| `lit-l4-4` | GPU | 4× L4 |
| `lit-a100-1` | GPU | 1× A100 |
| `lit-a100-8` | GPU | 8× A100 |
| `lit-a100-80gb-8` | GPU | 8× A100 80GB |
| `lit-h100-1` | GPU | 1× H100 |
| `lit-h100-8` | GPU | 8× H100 |
| `lit-h200x-1` | GPU | 1× H200 |
| `lit-h200x-8` | GPU | 8× H200 |
| `lit-b200x-8` | GPU | 8× B200 |

**Response:**

```json
{}
```

**Example:**

```bash
# Start on CPU-4 (default)
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/start" \
  -d '{"compute_config": {"name": "cpu-4", "spot": false}}'

# Start on A100 GPU
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/start" \
  -d '{"compute_config": {"name": "lit-a100-1", "spot": false}}'
```

---

## Stop Studio

**`POST /v1/projects/{projectId}/cloudspaces/{id}/stop`**

Stops a running Studio. The Studio enters a `Stopped` state and can be restarted later.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Request body:** None required.

**Response:**

```json
{}
```

**Example:**

```bash
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/stop"
```

---

## Get Studio Status

**`GET /v1/projects/{projectId}/cloudspaces/{id}/codestatus`**

Returns the current runtime status of a Studio instance.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Response:**

```json
{
  "in_use": {
    "phase": "CLOUD_SPACE_INSTANCE_STATE_RUNNING",
    "cloud_space_instance_id": "inst-abc123",
    "startup_status": {
      "initial_restore_finished": true,
      "top_up_restore_finished": true
    }
  },
  "requested": null
}
```

**Example:**

```bash
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/codestatus" \
  | jq '.in_use.phase'
```

---

## Switch Machine

**`PUT /v1/projects/{projectId}/cloudspaces/{id}/codeconfig`**

Switches a running Studio to a different machine type.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Request body:**

```json
{
  "compute_config": {
    "name": "lit-a100-1",
    "spot": false
  }
}
```

**Confirm the switch** (required after the machine is ready):

**`POST /v1/projects/{projectId}/cloudspaces/{id}/switch-confirm`**

**Cancel a pending switch:**

**`POST /v1/projects/{projectId}/cloudspaces/{id}/switch-cancel`**

---

## Execute Command

**`POST /v1/projects/{projectId}/cloudspaces/{id}/execute`**

Runs a command inside a running Studio and returns the output.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Request body:**

```json
{
  "command": "echo hello && ls -la",
  "session": "optional-session-id"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `command` | string | Yes | Shell command(s) to execute |
| `session` | string | No | Session ID for tracking long-running commands |

**Response:**

```json
{
  "output": "hello\ntotal 8\n...",
  "exit_code": 0,
  "session": "session-id-abc123"
}
```

**Example:**

```bash
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}/execute" \
  -d '{"command": "python --version"}'
```

---

## Get Long-Running Command Output

**`GET /v1/projects/{projectId}/cloudspaces/{id}/execute/{session}`**

Poll for the output of a long-running command identified by a session ID.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |
| `session` | string | Yes | Session ID returned from the execute endpoint |

---

## Stream Command Output

**`GET /v1/projects/{projectId}/cloudspaces/{id}/execute/{session}/stream`**

Stream the output of a long-running command (server-sent events).

---

## Delete Studio

**`DELETE /v1/projects/{projectId}/cloudspaces/{id}`**

Permanently deletes a Studio and all its data.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |

**Example:**

```bash
curl -s -X DELETE \
  -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/cloudspaces/${STUDIO_ID}"
```

---

## Update Studio

**`PUT /v1/projects/{projectId}/cloudspaces/{id}`**

Updates Studio properties such as display name, auto-sleep configuration, and environment variables.

**Request body:**

```json
{
  "display_name": "New Studio Name",
  "code_config": {
    "disable_auto_shutdown": false,
    "idle_shutdown_seconds": 3600,
    "env_vars": [
      {"name": "MY_VAR", "value": "my-value"}
    ]
  }
}
```

---

## Auto-Sleep Configuration

**`PUT /v1/projects/{projectId}/cloudspaces/{id}/sleepconfig`**

Configure auto-sleep settings for a Studio.

**Request body:**

```json
{
  "code_config": {
    "disable_auto_shutdown": false,
    "idle_shutdown_seconds": 3600
  }
}
```

| Field | Description |
|-------|-------------|
| `disable_auto_shutdown` | Set to `true` to disable auto-sleep (requires paid plan) |
| `idle_shutdown_seconds` | Seconds of inactivity before auto-sleep triggers |

---

## Duplicate (Fork) Studio

**`PUT /v1/projects/{projectId}/cloudspaces/{id}/fork`**

Duplicates a Studio (optionally to a different project).

**Request body:**

```json
{
  "project_id": "target-project-id",
  "compute_config": {
    "name": "cpu-4"
  },
  "new_name": "my-studio-copy"
}
```

---

## Restart Studio

**`POST /v1/projects/{projectId}/cloudspaces/{id}/restart`**

Restarts a running Studio instance without changing machine type.

---

## Keep Alive

**`POST /v1/projects/{projectId}/cloudspaces/{id}/keep-alive`**

Sends a keepalive heartbeat to prevent the Studio from auto-sleeping. The `lightning-sdk` sends this automatically every 30 seconds while a Studio is running.

---

## Plugins

### List Available Plugins

**`GET /v1/cloudspaces/plugins`**

Returns all plugins that can be installed in Studios.

### List Installed Plugins

**`GET /v1/projects/{projectId}/cloudspaces/{id}/plugins`**

Returns plugins installed in a specific Studio.

### Install Plugin

**`POST /v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}`**

Installs a plugin in the Studio.

**Path parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | Yes | Project ID |
| `id` | string | Yes | Studio ID |
| `pluginId` | string | Yes | Plugin name (e.g., `jobs`, `inference-server`, `custom-port`) |

**Built-in plugins:**

| Plugin ID | Description |
|-----------|-------------|
| `jobs` | Run async Jobs from the Studio |
| `multi-machine-training` | Multi-machine distributed training |
| `inference-server` | Deploy inference servers |
| `custom-port` | Expose custom ports |

### Uninstall Plugin

**`DELETE /v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}`**

### Execute Plugin

**`POST /v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}/execute`**

Runs a plugin command in the Studio.

---

## Endpoints (Ports)

### Add Port

**`POST /v1/projects/{projectId}/endpoints`**

Exposes a port on the Studio as a public HTTPS endpoint.

**Request body:**

```json
{
  "port": 8080,
  "name": "my-app",
  "cloudspace_id": "studio-id",
  "endpoint_type": "V1_ENDPOINT_TYPE_PRIVATE"
}
```

| Field | Description |
|-------|-------------|
| `port` | Port number to expose |
| `name` | Optional label for the endpoint |
| `endpoint_type` | `V1_ENDPOINT_TYPE_PRIVATE` or `V1_ENDPOINT_TYPE_PUBLIC` |

### List Ports

**`GET /v1/projects/{projectId}/endpoints`**

Returns all exposed ports for the project.

### Get Open Ports

**`GET /v1/projects/{projectId}/cloudspaces/{cloudspaceId}/open-ports`**

Returns currently open ports on the Studio instance.

### Remove Port

**`DELETE /v1/projects/{projectId}/endpoints/{id}`**

---

## Complete Python SDK Example

```python
import base64
import requests

BASE_URL = "https://lightning.ai"
USER_ID = "your-user-id"
API_KEY = "your-api-key"

def get_headers():
    token = base64.b64encode(f"{USER_ID}:{API_KEY}".encode()).decode()
    return {
        "Authorization": f"Basic {token}",
        "Content-Type": "application/json",
    }

def list_projects():
    # No GET /v1/projects — use memberships endpoint instead
    r = requests.get(
        f"{BASE_URL}/v1/memberships?filterByUserId=true",
        headers=get_headers()
    )
    return r.json()["memberships"]

def list_studios(project_id, user_id):
    # userId query param is required
    r = requests.get(
        f"{BASE_URL}/v1/projects/{project_id}/cloudspaces",
        params={"userId": user_id},
        headers=get_headers(),
    )
    return r.json()["cloudspaces"]

def start_studio(project_id, studio_id, machine="cpu-4"):
    r = requests.post(
        f"{BASE_URL}/v1/projects/{project_id}/cloudspaces/{studio_id}/start",
        headers=get_headers(),
        json={"compute_config": {"name": machine, "spot": False}},
    )
    return r.json()

def stop_studio(project_id, studio_id):
    r = requests.post(
        f"{BASE_URL}/v1/projects/{project_id}/cloudspaces/{studio_id}/stop",
        headers=get_headers(),
    )
    return r.json()

def run_command(project_id, studio_id, command):
    r = requests.post(
        f"{BASE_URL}/v1/projects/{project_id}/cloudspaces/{studio_id}/execute",
        headers=get_headers(),
        json={"command": command},
    )
    return r.json()

# Usage
memberships = list_projects()
project_id = memberships[0]["projectId"]  # note: response fields are camelCase

studios = list_studios(project_id, USER_ID)
studio_id = studios[0]["id"]

start_studio(project_id, studio_id, "cpu-4")
result = run_command(project_id, studio_id, "echo hello world")
print(result["output"])
stop_studio(project_id, studio_id)
```
