---
layout: default
title: Storage & Files
nav_order: 5
---

# Storage & Files API

Lightning AI provides storage for files, datasets, and model artifacts within Studios and Teamspaces.

---

## Studio File System (Direct Upload/Download)

Files can be uploaded to and downloaded from a running Studio via signed URLs.

### Upload File to Studio

To upload a file to a Studio, the SDK uses a multi-step process:

**Step 1: Get a signed upload URL**

**`POST /v1/projects/{projectId}/storage`**

**Request body:**

```json
{
  "cloudspace_id": "studio-id",
  "cluster_id": "lightning-cloud",
  "path": "path/to/file.py",
  "file_size": 1024
}
```

**Response:**

```json
{
  "upload_url": "https://storage.googleapis.com/bucket/...",
  "path": "path/to/file.py"
}
```

**Step 2: Upload using the signed URL**

```bash
curl -X PUT "${UPLOAD_URL}" \
  --upload-file /local/path/to/file.py
```

**Step 3: Notify completion**

**`POST /v1/projects/{projectId}/storage/complete`**

```json
{
  "cloudspace_id": "studio-id",
  "path": "path/to/file.py"
}
```

---

### Download File from Studio

**`GET /v1/projects/{projectId}/storage/download`**

**Query parameters:**

| Parameter | Description |
|-----------|-------------|
| `path` | File path within the Studio |
| `cloudspace_id` | Studio ID |
| `cluster_id` | Cloud account ID |

**Response:** Returns a signed download URL or redirects to the file.

---

### List Files / Get Metadata

**`GET /v1/projects/{projectId}/storage/metadata`**

Returns metadata for files in the Studio storage.

**Query parameters:**

| Parameter | Description |
|-----------|-------------|
| `cloudspace_id` | Studio ID |
| `cluster_id` | Cloud account ID |
| `path` | Directory path to list |

---

### Delete File

**`DELETE /v1/projects/{projectId}/storage`**

**Query parameters:**

| Parameter | Description |
|-----------|-------------|
| `cloudspace_id` | Studio ID |
| `cluster_id` | Cloud account ID |
| `path` | Path to delete |

---

### Multi-Part Upload (Large Files)

For files > 5 GB, use multi-part upload:

**Step 1: Start multi-part upload**

`POST /v1/projects/{projectId}/storage`

With `multipart: true` in the body.

**Step 2: Upload each part**

`POST /v1/projects/{projectId}/storage/uploads/{uploadId}`

**Step 3: Complete multi-part upload**

`POST /v1/projects/{projectId}/storage/complete`

---

## Folder Index

### Get Folder Index

**`GET /v1/projects/{projectId}/cloudspaces/{id}/folder-index`**

Returns a file tree for the Studio's working directory.

**Response:**

```json
{
  "files": [
    {
      "path": "train.py",
      "size": 2048,
      "modified_at": "2024-01-15T10:00:00Z"
    }
  ]
}
```

### Refresh Folder Index

**`POST /v1/projects/{projectId}/cloudspaces/{id}/index`**

Forces a re-scan of the Studio's file system.

---

## Artifact Pages

**`GET /v1/projects/{projectId}/cloudspaces/artifacts/{id}/page`**

Returns paginated artifacts for a Studio.

**`GET /v1/projects/{projectId}/storage/uploads/artifacts/page`**

Returns paginated upload artifacts.

---

## Teamspace Storage (Drive)

### Upload File to Teamspace Drive

**`POST /v1/projects/temporary_storage`**

Uploads a file to teamspace-shared storage (accessible by all Studios in the project).

**`POST /v1/projects/temporary_storage/uploads/{uploadId}`**

Multi-part upload parts for teamspace storage.

**`POST /v1/projects/temporary_storage/complete`**

Complete multi-part upload for teamspace storage.

---

## Storage Transfers

Move or copy data between storage locations.

**Base path:** `/v1/projects/{projectId}/storage-transfers`

### Create Transfer

**`POST /v1/projects/{projectId}/storage-transfers`**

**Request body:**

```json
{
  "source": {
    "path": "/source/path",
    "cloudspace_id": "source-studio-id"
  },
  "destination": {
    "path": "/dest/path",
    "cloudspace_id": "dest-studio-id"
  }
}
```

### List Transfers

**`GET /v1/projects/{projectId}/storage-transfers`**

### Get Transfer

**`GET /v1/projects/{projectId}/storage-transfers/{id}`**

### Pause Transfer

**`POST /v1/projects/{projectId}/storage-transfers/{id}/pause`**

### Resume Transfer

**`POST /v1/projects/{projectId}/storage-transfers/{id}/resume`**

### Abort Transfer

**`POST /v1/projects/{projectId}/storage-transfers/{id}/abort`**

### Validate Transfer

**`POST /v1/projects/{projectId}/storage-transfers/validate`**

---

## Datasets (Lit Datasets)

Lightning AI has a dataset versioning system for managing ML training data.

**Base path:** `/v1/projects/{projectId}/lit-datasets`

### Create Dataset

**`POST /v1/projects/{projectId}/lit-datasets`**

### List Datasets

**`GET /v1/projects/{projectId}/lit-datasets`**

### Get Dataset by Name

**`GET /v1/projects/{projectOwnerName}/{projectName}/lit-datasets/{datasetName}`**

### Get Dataset

**`GET /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}`**

### Delete Dataset

**`DELETE /v1/projects/{projectId}/lit-datasets/{datasetId}`**

### Update Dataset

**`PUT /v1/projects/{projectId}/lit-datasets/{datasetId}`**

### Set Dataset Visibility

**`PUT /v1/projects/{projectId}/lit-datasets/{datasetId}/visibility`**

### Dataset Versions

**Create version:**
`POST /v1/projects/{projectId}/lit-datasets/{datasetId}/versions`

**List versions:**
`GET /v1/projects/{projectId}/lit-datasets/{datasetId}/versions`

**Get version files:**
`GET /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/files`

**Set default version:**
`PUT /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/default`

**Delete version:**
`DELETE /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}`

### Dataset Upload (Multi-part)

**Start upload:**
`POST /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads`

**Upload parts:**
`POST /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads/{uploadId}/parts`

**Complete upload:**
`POST /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads/{uploadId}/complete`

**Complete version:**
`POST /v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/complete`

---

## Models Store

Store and version ML models.

**Base path:** `/v1/projects/{projectId}/models`

### Create Model

**`POST /v1/projects/{projectId}/models`**

### List Models

**`GET /v1/projects/{projectId}/models`**

**`GET /v1/models`** — List all public models

### Get Model

**`GET /v1/projects/{projectId}/models/{modelId}`**

### Get Model by Name

**`GET /v1/projects/{projectOwnerName}/{projectName}/models/{modelName}`**

### Delete Model

**`DELETE /v1/projects/{projectId}/models/{modelId}`**

### Update Model

**`PUT /v1/projects/{projectId}/models/{modelId}`**

### Set Model Visibility

**`PUT /v1/projects/{projectId}/models/{modelId}/visibility`**

### Model Versions

**Create version:**
`POST /v1/projects/{projectId}/models/{modelId}/versions`

**List versions:**
`GET /v1/projects/{projectId}/models/{modelId}/versions`

**Get version:**
`GET /v1/projects/{projectId}/models/{modelId}/versions/{version}`

**Get version file:**
`GET /v1/projects/{projectId}/models/{modelId}/versions/{version}/file`

**Get version files:**
`GET /v1/projects/{projectId}/models/{modelId}/versions/{version}/files`

**Set default version:**
`PUT /v1/projects/{projectId}/models/{modelId}/versions/{version}/default`

**Delete version:**
`DELETE /v1/projects/{projectId}/models/{modelId}/versions/{version}`

### Model Upload (Multi-part)

**Start upload:**
`POST /v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads`

**Upload parts:**
`POST /v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads/{uploadId}/parts`

**Complete upload:**
`POST /v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads/{uploadId}/complete`

**Complete version:**
`POST /v1/projects/{projectId}/models/{modelId}/versions/{version}/complete`

---

## Secrets

Secrets are encrypted key-value pairs injected as environment variables into Studios and Jobs.

**Base paths:**
- `/v1/projects/{projectId}/secrets` — Project-scoped secrets
- `/v1/secrets` — User-scoped secrets

### Create Secret (Project)

**`POST /v1/projects/{projectId}/secrets`**

**Request body:**

```json
{
  "name": "MY_SECRET",
  "value": "super-secret-value"
}
```

### List Secrets (Project)

**`GET /v1/projects/{projectId}/secrets`**

Returns secret names (not values).

### Get Secret

**`GET /v1/projects/{projectId}/secrets/{id}`**

### Update Secret

**`PUT /v1/projects/{projectId}/secrets/{id}`**

### Delete Secret

**`DELETE /v1/projects/{projectId}/secrets/{id}`**

### User-Scoped Secrets

**Create:** `POST /v1/secrets`
**List:** `GET /v1/secrets`
**Get:** `GET /v1/secrets/{id}`
**Update:** `PUT /v1/secrets/{id}`
**Delete:** `DELETE /v1/secrets/{id}`

---

## Data Connections

Data Connections link external data sources (S3, GCS, etc.) to your Teamspace.

**Base path:** `/v1/projects/{projectId}/data-connections`

### Create Data Connection

**`POST /v1/projects/{projectId}/data-connections`**

### List Data Connections

**`GET /v1/projects/{projectId}/data-connections`**

### Get Data Connection

**`GET /v1/projects/{projectId}/data-connections/{id}`**

### Delete Data Connection

**`DELETE /v1/projects/{projectId}/data-connections/{id}`**

### Update Data Connection

**`PUT /v1/projects/{projectId}/data-connections/{id}`**

### Get Folder Index

**`GET /v1/projects/{projectId}/data-connections/{id}/folder-index`**

### Get Temp Credentials

**`GET /v1/projects/{projectId}/data-connections/{id}/temp-bucket-credentials`**

Returns temporary cloud provider credentials for direct access.

### Validate Data Connection

**`POST /v1/projects/{projectId}/data-connections/valid`**

### Setup Data Connection

**`POST /v1/projects/{projectId}/data-connections/setup`**

### Re-index Data Connection

**`POST /v1/projects/{projectId}/data-connections/{id}/index`**

---

## File System View

The file system service provides a unified view of all storage types:

**`GET /v1/filesystem/{projectId}/cloudspaces/{currentId}`** — Studio files
**`GET /v1/filesystem/{projectId}/datasets/{currentId}`** — Dataset files
**`GET /v1/filesystem/{projectId}/jobs/{currentId}`** — Job artifacts
**`GET /v1/filesystem/{projectId}/mmts/{currentId}`** — MMT artifacts
**`GET /v1/filesystem/{projectId}/volumes`** — Volumes
**`PUT /v1/filesystem/{projectId}/volumes`** — Update volumes

---

## Python SDK Upload Example

```python
from lightning_sdk import Studio

studio = Studio(
    name="my-studio",
    teamspace="my-team",
    org="my-org",
)

# Make sure studio is running
studio.start()

# Upload a single file
studio.upload_file("local_script.py", remote_path="scripts/train.py")

# Upload an entire folder
studio.upload_folder("./my_project", remote_path="my_project")

# Download a file
studio.download_file("outputs/model.pt", file_path="./model.pt")

# Download a folder
studio.download_folder("outputs/", target_path="./outputs")

# Run a command after uploading
output = studio.run("python scripts/train.py")
print(output)
```
