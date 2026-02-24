---
layout: default
title: Jobs & Deployments
nav_order: 4
---

# Jobs & Deployments API

Lightning AI supports running async workloads as **Jobs** (single machine) or **Multi-Machine Training (MMT)** jobs, and deploying models as persistent **Deployments** (inference servers).

---

## Jobs

Jobs run async compute workloads on a specified machine type. They are typically created from a Studio's code snapshot.

**Base path:** `/v1/projects/{projectId}/jobs`

### List Jobs

**`GET /v1/projects/{projectId}/jobs`**

Returns all jobs in a project.

**Query parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `phase` | string | Filter by status phase |
| `name` | string | Filter by job name |
| `cloudspace_id` | string | Filter by Studio ID |

**Response:**

```json
{
  "jobs": [
    {
      "id": "job-id-001",
      "name": "my-training-job",
      "status": {
        "phase": "JOB_STATE_RUNNING"
      },
      "spec": {
        "cluster_id": "lightning-cloud",
        "requested_compute": {
          "name": "lit-a100-1"
        }
      },
      "created_at": "2024-01-15T10:00:00Z"
    }
  ]
}
```

**Job status phases:**

| Phase | Description |
|-------|-------------|
| `JOB_STATE_PENDING` | Job is queued |
| `JOB_STATE_RUNNING` | Job is executing |
| `JOB_STATE_SUCCEEDED` | Job completed successfully |
| `JOB_STATE_FAILED` | Job failed |
| `JOB_STATE_STOPPED` | Job was manually stopped |

**Example:**

```bash
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/jobs" | jq '.jobs[].name'
```

---

### Get Job

**`GET /v1/projects/{projectId}/jobs/{id}`**

Returns a single job by ID.

```bash
curl -s -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/jobs/${JOB_ID}"
```

---

### Get Job by Name

**`GET /v1/projects/{projectOwnerName}/{projectName}/jobs/{jobName}`**

Returns a job by its human-readable name.

---

### Create Job

**`POST /v1/projects/{projectId}/jobs`**

Submits a new async job.

**Request body:**

```json
{
  "name": "my-training-job",
  "spec": {
    "cluster_id": "lightning-cloud",
    "requested_compute": {
      "name": "lit-a100-1",
      "spot": false
    },
    "lightningapp_instance_id": "studio-id",
    "run": {
      "kind": "LIGHTNINGAPP_INSTANCE",
      "entrypoint_command": "python train.py --epochs 10",
      "env": [
        {"name": "BATCH_SIZE", "value": "32"}
      ]
    }
  }
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Unique job name within the project |
| `spec.cluster_id` | string | No | Cloud account ID |
| `spec.requested_compute.name` | string | Yes | Machine slug (e.g., `lit-a100-1`) |
| `spec.requested_compute.spot` | boolean | No | Use interruptible instances |
| `spec.lightningapp_instance_id` | string | No | Studio ID to snapshot for the job |
| `spec.run.entrypoint_command` | string | Yes | Command to execute |
| `spec.run.env` | array | No | Environment variable overrides |

**Response:** Returns the created job object.

**Example:**

```bash
curl -s -X POST \
  -H "Authorization: Basic ${AUTH}" \
  -H "Content-Type: application/json" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/jobs" \
  -d '{
    "name": "train-resnet",
    "spec": {
      "requested_compute": {"name": "lit-a100-1"},
      "lightningapp_instance_id": "'${STUDIO_ID}'",
      "run": {
        "entrypoint_command": "python train.py"
      }
    }
  }'
```

---

### Update Job

**`PUT /v1/projects/{projectId}/jobs/{id}`**

Updates a job (e.g., stop it by updating its desired phase).

---

### Delete Job

**`DELETE /v1/projects/{projectId}/jobs/{id}`**

Deletes a completed or failed job and its artifacts.

```bash
curl -s -X DELETE \
  -H "Authorization: Basic ${AUTH}" \
  "https://lightning.ai/v1/projects/${PROJECT_ID}/jobs/${JOB_ID}"
```

---

### Get Job Logs

**`GET /v1/projects/{projectId}/jobs/{id}/page-logs`**

Returns paginated logs from a job.

**Query parameters:**

| Parameter | Description |
|-----------|-------------|
| `cursor` | Pagination cursor |
| `limit` | Max number of log lines |

**Download Logs:**

**`GET /v1/projects/{projectId}/jobs/{id}/download-logs`**

Returns a URL to download the full log file.

---

### Get Job System Metrics

**`GET /v1/projects/{projectId}/jobs/system-metrics`**

Returns CPU/GPU/memory metrics for jobs in the project.

---

## Multi-Machine Training (MMT)

MMT jobs run the same command across multiple machines simultaneously (e.g., for distributed training with `torchrun`).

**Base path:** `/v1/projects/{projectId}/multi-machine-jobs`

### Create MMT Job

**`POST /v1/projects/{projectId}/multi-machine-jobs`**

**Request body:**

```json
{
  "name": "distributed-training",
  "spec": {
    "num_machines": 4,
    "cluster_id": "lightning-cloud",
    "requested_compute": {
      "name": "lit-h100-8",
      "spot": false
    },
    "lightningapp_instance_id": "studio-id",
    "run": {
      "entrypoint_command": "torchrun --nproc_per_node=8 --nnodes=4 train.py"
    }
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `spec.num_machines` | integer | Number of machines to use |

### List MMT Jobs

**`GET /v1/projects/{projectId}/multi-machine-jobs`**

### Get MMT Job

**`GET /v1/projects/{projectId}/multi-machine-jobs/{id}`**

### Get MMT Job by Name

**`GET /v1/projects/{projectId}/multi-machine-jobs/{name}/getbyname`**

### Get MMT Job Events

**`GET /v1/projects/{projectId}/multi-machine-jobs/{id}/events`**

### Delete MMT Job

**`DELETE /v1/projects/{projectId}/multi-machine-jobs/{id}`**

### Update MMT Job

**`PUT /v1/projects/{projectId}/multi-machine-jobs/{id}`**

---

## Deployments (Inference Servers)

Deployments run persistent services (e.g., model inference APIs). They auto-scale based on traffic.

**Base path:** `/v1/projects/{projectId}/deployments`

### List Deployments

**`GET /v1/projects/{projectId}/deployments`**

Returns all deployments in a project.

---

### Get Deployment

**`GET /v1/projects/{projectId}/deployments/{id}`**

---

### Get Deployment by Name

**`GET /v1/projects/{projectId}/deployments/{name}/getbyname`**

---

### Get Deployment by Owner/Project/Name

**`GET /v1/projects/{projectOwnerName}/{projectName}/deployments/{deploymentName}`**

---

### Create Deployment

**`POST /v1/projects/{projectId}/deployments`**

**Request body:**

```json
{
  "name": "my-llm-server",
  "spec": {
    "cluster_id": "lightning-cloud",
    "requested_compute": {
      "name": "lit-a100-1"
    },
    "work": {
      "image": "ghcr.io/my-org/my-llm:latest",
      "env": [
        {"name": "MODEL_PATH", "value": "/models/llm"}
      ]
    },
    "min_replicas": 1,
    "max_replicas": 3
  }
}
```

---

### Delete Deployment

**`DELETE /v1/projects/{projectId}/deployments/{id}`**

---

### Update Deployment

**`PUT /v1/projects/{projectId}/deployments/{id}`**

Update deployment configuration (e.g., replicas, environment variables).

---

### Get Deployment Status

**`GET /v1/projects/{projectId}/deployments/{id}/status`**

Returns the current status and replica count.

---

### Duplicate Deployment

**`POST /v1/projects/{projectId}/deployments/{sourceDeploymentId}/duplicate`**

Creates a copy of an existing deployment.

---

### Get Deployment Telemetry

**`GET /v1/projects/{projectId}/deployments/{id}/telemetry`**

Returns request/response metrics.

**`GET /v1/projects/{projectId}/deployments/{id}/telemetry-aggregated`**

Returns aggregated telemetry.

---

### Deployment Alerting

**Create alerting policy:**
`POST /v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies`

**List alerting policies:**
`GET /v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies`

**Update alerting policy:**
`PUT /v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies`

**Delete alerting policy:**
`DELETE /v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies/{id}`

---

### Deployment Releases

**List releases:**
`GET /v1/projects/{projectId}/deployments/{deploymentId}/releases`

**Get release:**
`GET /v1/projects/{projectId}/deployments/{deploymentId}/releases/{id}`

**Create release:**
`POST /v1/projects/{projectId}/deployments/{deploymentId}/releases/{id}`

---

## Validate Job/Deployment

**`POST /v1/deployments/validate`**

Validates a job or deployment spec before submitting.

---

## Studio Jobs (Serverless)

Studio Jobs are lighter-weight jobs that run within Studio infrastructure.

**Base path:** `/v1/projects/{projectId}/studioapp/jobs`

### List Studio Jobs

**`GET /v1/projects/{projectId}/studioapp/jobs`**

### Get Studio Job

**`GET /v1/projects/{projectId}/studioapp/jobs/{id}`**

### Create Studio Job

**`POST /v1/projects/{projectId}/studioapp/jobs`**

### Stop Studio Job

**`POST /v1/projects/{projectId}/studioapp/jobs/{id}/stop`**

### Delete Studio Job

**`DELETE /v1/projects/{projectId}/studioapp/jobs/{id}`**

---

## Python SDK Usage

The `lightning-sdk` provides high-level wrappers for Jobs:

```python
from lightning_sdk import Studio, Machine

# Connect to an existing Studio
studio = Studio(
    name="my-studio",
    teamspace="my-team",
    org="my-org"  # or user="username"
)

# Run a simple job
job = studio.run_job(
    name="train-model",
    machine=Machine.A100,
    command="python train.py --epochs 100",
    env={"BATCH_SIZE": "64"},
    interruptible=False,
)

# Wait for it and check status
print(job.status)

# Run multi-machine training
mmt = studio.run_mmt(
    name="distributed-train",
    num_machines=4,
    machine=Machine.H100_X_8,
    command="torchrun --nproc_per_node=8 --nnodes=4 train.py",
)
```

Alternatively, use the `Job` class directly:

```python
from lightning_sdk import Job, Machine, Studio, Teamspace

job = Job.run(
    name="my-job",
    machine=Machine.A100,
    command="python train.py",
    studio=Studio(name="my-studio", teamspace="my-team"),
)
print(f"Job status: {job.status}")
```

---

## Schedules

Jobs can be run on a schedule using cron expressions.

**Base path:** `/v1/projects/{projectId}/schedules`

### Create Schedule

**`POST /v1/projects/{projectId}/schedules`**

**Request body:**

```json
{
  "name": "nightly-training",
  "cron_expression": "0 2 * * *",
  "cloudspace_id": "studio-id",
  "command": "python train.py"
}
```

### List Schedules

**`GET /v1/projects/{projectId}/schedules`**

### Get Schedule

**`GET /v1/projects/{projectId}/schedules/{id}`**

### Delete Schedule

**`DELETE /v1/projects/{projectId}/schedules/{id}`**

### Update Schedule

**`PUT /v1/projects/{projectId}/schedules/{id}`**

### List Schedule Runs

**`GET /v1/projects/{projectId}/cloudspaces/{cloudspaceId}/schedules/{scheduleId}/runs`**

### Create Schedule Run (trigger manually)

**`POST /v1/projects/{projectId}/cloudspaces/{cloudspaceId}/schedules/{scheduleId}/runs`**
