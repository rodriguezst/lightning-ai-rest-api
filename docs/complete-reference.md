---
layout: default
title: Complete API Reference
nav_order: 6
---

# Complete API Reference

This page lists all 638 REST API endpoints across 48 service groups, extracted from `lightning-sdk` v2026.2.6.

**Base URL:** `https://lightning.ai`

**Authentication:** All endpoints require `Authorization: Basic {base64(userId:apiKey)}` or `Authorization: Bearer {jwt-token}` unless noted.

---

## Quick Navigation

- [Authentication](#authentication-service)
- [Studios (CloudSpaces)](#cloudspace-service)
- [Environment Templates](#cloudspace-environment-template-service)
- [Projects (Teamspaces)](#projects-service)
- [Jobs](#jobs-service)
- [Deployments](#deployments-jobs-service)
- [Multi-Machine Training](#jobs-service)
- [Storage](#storage-service)
- [Datasets](#dataset-service-lit-datasets)
- [Models](#models-store)
- [Secrets](#secret-service)
- [Clusters](#cluster-service)
- [Organizations](#organizations-service)
- [SSH Keys](#ssh-public-key-service)
- [Pipelines](#pipelines-service)
- [Schedules](#schedules-service)
- [Endpoints (Ports)](#endpoint-service)
- [Agents (AI Assistants)](#agents-assistants-service)
- [Users](#user-service)
- [Billing](#billing-service)

---

## Authentication Service

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/auth/user` | Get current user |
| `PUT` | `/v1/auth/user` | Update current user |
| `GET` | `/v1/auth/user/notification-preferences` | Get notification preferences |
| `PUT` | `/v1/auth/user/notification-preferences` | Update notification preferences |
| `POST` | `/v1/auth/guest-login` | Create guest session |
| `POST` | `/v1/auth/login` | Login with API key (returns JWT) |
| `POST` | `/v1/auth/logout` | Logout |
| `POST` | `/v1/auth/magic-link-login` | Login via magic link |
| `POST` | `/v1/auth/refresh` | Refresh JWT token |
| `PUT` | `/v1/auth/reset-api-key` | Reset API key |
| `POST` | `/v1/auth/token-login` | Login with auth token key |
| `GET` | `/v1/platform-notifications` | Get platform notifications |

---

## CloudSpace Service

Studios (CloudSpaces) are the core resource. `{projectId}` = Teamspace ID, `{id}` = Studio ID.

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/projects/{projectId}/cloudspaces` | List Studios |
| `POST` | `/v1/projects/{projectId}/cloudspaces` | Create Studio |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}` | Get Studio |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}` | Update Studio |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{id}` | Delete Studio |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/start` | Start Studio |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/stop` | Stop Studio |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/restart` | Restart Studio |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/codestatus` | Get Studio status |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/codeconfig` | Get instance config |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/codeconfig` | Switch machine (update instance config) |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/sleepconfig` | Update auto-sleep config |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/switch-confirm` | Confirm machine switch |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/switch-cancel` | Cancel machine switch |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/execute` | Execute command |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/execute/{session}` | Get long-running command output |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/execute/{session}/stream` | Stream command output |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/keep-alive` | Send keepalive |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/fork` | Duplicate Studio |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/engage` | Engage Studio |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/ide` | Update IDE config |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/restart` | Restart instance |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/invalidate-code-settings` | Invalidate code settings |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/collaborate` | Update collab settings |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/visibility` | Set visibility |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/open-ports` | Get open ports |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/size` | Get storage size |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/required-balance-status` | Check balance |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/folder-index` | Get file listing |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/index` | Refresh folder index (POST) |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/index` | Update index |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{id}/index` | Delete index |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/refresh-path` | Refresh path |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/transfer/duration` | Get transfer duration estimate |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/transfer` | Transfer Studio |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/cancel-transfer` | Cancel transfer |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/complete-transfer` | Complete transfer |
| `GET` | `/v1/projects/{projectId}/cloudspaces/artifacts/{id}/page` | Get artifact page |
| `GET` | `/v1/projects/{projectId}/cloudspaces/cold-start-stats` | Get cold start stats |
| `GET` | `/v1/projects/{projectId}/cloudspaces/cold-start-metrics` | Get cold start metrics |
| `GET` | `/v1/projects/{projectId}/system-metrics-aggregated` | Get system metrics |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/system-metrics` | Report system metrics |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/report-idle-state` | Report idle state |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/report-stop-at` | Report stop time |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/python-versions` | List Python versions |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/metric` | Post instance metric |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/should-start-syncing` | Check sync status |
| **Plugins** | | |
| `GET` | `/v1/cloudspaces/plugins` | List available plugins |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/plugins` | List installed plugins |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}` | Install plugin |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}` | Uninstall plugin |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/plugins/{pluginId}/execute` | Execute plugin |
| **Sessions** | | |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/sessions` | Create session |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/sessions` | List sessions |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/sessions/{id}/execute` | Execute in session |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/sessions/{id}` | Delete session |
| **Runs** | | |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs` | Create Lightning Run |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs` | List runs |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs/{id}` | Get run |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs/{id}/get` | Get run (POST) |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs/{id}` | Create run instance |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs/{id}` | Delete run |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/runs/{runId}/source-code` | Get source code URL |
| **Versions** | | |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions` | Create version |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions` | List versions |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions/{id}` | Get version |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions/{id}` | Update version |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions/{id}` | Delete version |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions/{id}/artifact-page` | Get version artifacts |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/versions/{id}/folder-index` | Get version files |
| **Publications** | | |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{id}/publications` | Publish Studio |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{id}/publications` | List publications |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{id}/publications` | Update publication |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{id}/publications` | Unpublish Studio |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/version-publication` | Get version publication |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/version-publications` | Create version publication |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/version-publications` | List version publications |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/version-publications` | Update version publication |
| `DELETE` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/version-publications` | Delete version publication |
| **Apps** | | |
| `POST` | `/v1/cloudspaces/apps` | Create app |
| `GET` | `/v1/cloudspaces/apps` | List apps |
| `GET` | `/v1/cloudspaces/apps/{id}` | Get app |
| `PUT` | `/v1/cloudspaces/apps/{id}` | Update app |
| `DELETE` | `/v1/cloudspaces/apps/{id}` | Delete app |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/apps/{id}` | Create app instance |
| `PUT` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/apps/{id}/fork` | Fork app instance |
| **Public / Discovery** | | |
| `GET` | `/v1/cloudspaces` | List all (bulk) |
| `GET` | `/v1/cloudspaces/active` | List active instances |
| `GET` | `/v1/cloudspaces/published` | List published Studios |
| `GET` | `/v1/cloudspaces/tags` | List available tags |
| `GET` | `/v1/cloudspaces/public/{id}` | Get public Studio |
| `GET` | `/v1/collab-sessions/{collabSessionId}/cloudspace` | Get by collab session |
| `GET` | `/v1/users/{userName}/projects/{projectName}/cloudspaces/{cloudspaceName}/getbyname` | Get by name |
| `PUT` | `/v1/users/{userName}/projects/{projectName}/cloudspaces/{cloudspaceName}/request-access` | Request access |
| `GET` | `/v1/check-external-service-status` | Health check |

---

## CloudSpace Environment Template Service

Environment templates define base Studio configurations (pre-installed packages, system dependencies).

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/cloudspaces/environment-templates` | Create template |
| `GET` | `/v1/cloudspaces/environment-templates` | List templates |
| `GET` | `/v1/cloudspaces/environment-templates/managed` | List managed templates |
| `GET` | `/v1/cloudspaces/environment-templates/{id}` | Get template |
| `PUT` | `/v1/cloudspaces/environment-templates/{id}` | Update template |
| `DELETE` | `/v1/cloudspaces/environment-templates/{id}` | Delete template |

---

## Projects Service

Projects are Teamspaces — the organizational unit that owns Studios, Jobs, and Storage.

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects` | Create project |
| `GET` | `/v1/projects/{id}` | Get project |
| `PUT` | `/v1/projects/{id}` | Update project |
| `DELETE` | `/v1/projects/{id}` | Delete project |
| `POST` | `/v1/projects/{projectId}` | Update project (POST) |
| `GET` | `/v1/memberships` | Get my memberships |
| `PUT` | `/v1/projects/{projectId}/tab-order` | Update tab order |
| **Memberships** | | |
| `POST` | `/v1/projects/{projectId}/memberships` | Add member |
| `GET` | `/v1/projects/{projectId}/memberships` | List members |
| `DELETE` | `/v1/projects/{projectId}/memberships/{userId}` | Remove member |
| `POST` | `/v1/projects/{projectId}/memberships/{userId}/membershiprolebindings` | Add role to member |
| `GET` | `/v1/projects/{projectId}/memberships/{userId}/membershiprolebindings` | Get member roles |
| `DELETE` | `/v1/projects/{projectId}/memberships/{userId}/membershiprolebindings/{roleId}` | Remove member role |
| **Roles** | | |
| `GET` | `/v1/projects/{projectId}/roles/{id}` | Get role |
| `GET` | `/v1/projects/{projectId}/roles` | List roles |
| `DELETE` | `/v1/projects/{projectId}/roles/{id}` | Delete role |
| `POST` | `/v1/projects/{projectId}/invite` | Invite user |
| **Cluster Bindings** | | |
| `POST` | `/v1/projects/{projectId}/projectclustersbindings` | Add cluster binding |
| `GET` | `/v1/projects/{projectId}/projectclustersbindings` | List cluster bindings |
| `DELETE` | `/v1/projects/{projectId}/projectclustersbindings/{clusterId}` | Remove cluster binding |

---

## Jobs Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/jobs` | Create job |
| `GET` | `/v1/projects/{projectId}/jobs` | List jobs |
| `GET` | `/v1/projects/{projectId}/jobs/{id}` | Get job |
| `PUT` | `/v1/projects/{projectId}/jobs/{id}` | Update job |
| `DELETE` | `/v1/projects/{projectId}/jobs/{id}` | Delete job |
| `GET` | `/v1/projects/{projectId}/jobs/find` | Find jobs |
| `GET` | `/v1/projects/{projectOwnerName}/{projectName}/jobs/{jobName}` | Get job by name |
| `GET` | `/v1/projects/{projectId}/jobs/{id}/page-logs` | Get job logs (paginated) |
| `GET` | `/v1/projects/{projectId}/jobs/{id}/download-logs` | Download job logs |
| `GET` | `/v1/projects/{projectId}/jobs/logs` | Get logs |
| `GET` | `/v1/projects/{projectId}/jobs/system-metrics` | Get system metrics |
| `POST` | `/v1/projects/{projectId}/jobs/{jobId}/system-metrics` | Post system metrics |
| `PUT` | `/v1/projects/{projectId}/jobs/{id}/visibility` | Set visibility |
| `PUT` | `/v1/projects/{projectId}/jobs/{id}/report-logs-activity` | Report logs activity |
| `PUT` | `/v1/projects/{projectId}/jobs/{id}/report-restart-timings` | Report restart timings |
| `POST` | `/v1/projects/{projectId}/jobs/{id}/timings` | Post timings |
| `POST` | `/v1/projects/{projectId}/jobs/{id}/index` | Index job |
| `PUT` | `/v1/projects/{projectId}/jobs/{id}/index` | Update job index |
| `DELETE` | `/v1/projects/{projectId}/jobs/{id}/index` | Delete job index |
| `GET` | `/v1/jobs` | List all jobs |
| `GET` | `/v1/jobs/all` | List all jobs (admin) |
| `GET` | `/v1/stats/jobs` | Job statistics |
| **Multi-Machine Training** | | |
| `POST` | `/v1/projects/{projectId}/multi-machine-jobs` | Create MMT job |
| `GET` | `/v1/projects/{projectId}/multi-machine-jobs` | List MMT jobs |
| `GET` | `/v1/projects/{projectId}/multi-machine-jobs/{id}` | Get MMT job |
| `PUT` | `/v1/projects/{projectId}/multi-machine-jobs/{id}` | Update MMT job |
| `DELETE` | `/v1/projects/{projectId}/multi-machine-jobs/{id}` | Delete MMT job |
| `GET` | `/v1/projects/{projectId}/multi-machine-jobs/{name}/getbyname` | Get MMT job by name |
| `GET` | `/v1/projects/{projectId}/multi-machine-jobs/{id}/events` | Get MMT job events |
| **Deployments** | | |
| `POST` | `/v1/projects/{projectId}/deployments` | Create deployment |
| `GET` | `/v1/projects/{projectId}/deployments` | List deployments |
| `GET` | `/v1/projects/{projectId}/deployments/{id}` | Get deployment |
| `PUT` | `/v1/projects/{projectId}/deployments/{id}` | Update deployment |
| `DELETE` | `/v1/projects/{projectId}/deployments/{id}` | Delete deployment |
| `GET` | `/v1/projects/{projectId}/deployments/{name}/getbyname` | Get deployment by name |
| `GET` | `/v1/projects/{projectOwnerName}/{projectName}/deployments/{deploymentName}` | Get by owner/name |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/status` | Get deployment status |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/telemetry` | Get telemetry |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/telemetry-aggregated` | Get aggregated telemetry |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/events` | Get events |
| `PUT` | `/v1/projects/{projectId}/deployments/{id}/visibility` | Set visibility |
| `POST` | `/v1/projects/{projectId}/deployments/{sourceDeploymentId}/duplicate` | Duplicate deployment |
| `POST` | `/v1/deployments/validate` | Validate deployment spec |
| `GET` | `/v1/orgs/{orgId}/deployments/{deploymentId}` | Get org deployment |
| `GET` | `/v1/orgs/{orgId}/deployments` | List org deployments |
| `PUT` | `/v1/orgs/{orgId}/deployments/{deploymentId}` | Update org deployment |
| `GET` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies` | List alerting policies |
| `POST` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies` | Create alerting policy |
| `PUT` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies` | Update alerting policy |
| `DELETE` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-policies/{id}` | Delete alerting policy |
| `GET` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-events` | List alerting events |
| `PUT` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-events/{id}` | Update alerting event |
| `PUT` | `/v1/projects/{projectId}/deployments/{deploymentId}/alerting-events` | Bulk update events |
| `GET` | `/v1/projects/{projectId}/deployments/{deploymentId}/releases` | List releases |
| `GET` | `/v1/projects/{projectId}/deployments/{deploymentId}/releases/{id}` | Get release |
| `POST` | `/v1/projects/{projectId}/deployments/{deploymentId}/releases/{id}` | Create release |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/routing-telemetry-content` | Get routing telemetry |
| `GET` | `/v1/projects/{projectId}/deployments/{id}/list-routing-telemetry` | List routing telemetry |
| `PUT` | `/v1/projects/{projectId}/deployment/{deploymentId}/jobs/{jobId}/report-routing-telemetry` | Report routing telemetry |

---

## Studio Jobs Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/studioapp/jobs` | Create studio job |
| `GET` | `/v1/projects/{projectId}/studioapp/jobs` | List studio jobs |
| `GET` | `/v1/projects/{projectId}/studioapp/jobs/{id}` | Get studio job |
| `POST` | `/v1/projects/{projectId}/studioapp/jobs/{id}` | Update studio job |
| `POST` | `/v1/projects/{projectId}/studioapp/jobs/{id}/stop` | Stop studio job |
| `DELETE` | `/v1/projects/{projectId}/studioapp/jobs/{id}` | Delete studio job |

---

## Schedules Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/schedules` | Create schedule |
| `GET` | `/v1/projects/{projectId}/schedules` | List schedules |
| `GET` | `/v1/projects/{projectId}/schedules/{id}` | Get schedule |
| `PUT` | `/v1/projects/{projectId}/schedules/{id}` | Update schedule |
| `DELETE` | `/v1/projects/{projectId}/schedules/{id}` | Delete schedule |
| `POST` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/schedules/{scheduleId}/runs` | Trigger schedule run |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/schedules/{scheduleId}/runs` | List schedule runs |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudspaceId}/schedules/{scheduleId}/runs/{runId}` | Get schedule run |

---

## Storage Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/storage` | Upload file (get signed URL) |
| `GET` | `/v1/projects/{projectId}/storage` | Get storage contents |
| `DELETE` | `/v1/projects/{projectId}/storage` | Delete file |
| `GET` | `/v1/projects/{projectId}/storage/download` | Download file |
| `GET` | `/v1/projects/{projectId}/storage/metadata` | Get file metadata |
| `POST` | `/v1/projects/{projectId}/storage/complete` | Complete upload |
| `POST` | `/v1/projects/{projectId}/storage/uploads/{uploadId}` | Upload part |
| `GET` | `/v1/projects/{projectId}/storage/uploads/artifacts/page` | Get upload artifacts |
| `GET` | `/v1/projects/{projectId}/storage/uploads/folder-index` | Get upload folder index |
| `GET` | `/v1/orgs/{orgId}/storage/metadata` | Get org storage metadata |
| `GET` | `/v1/projects/{projectId}/locked-resources` | Get locked resources |
| `POST` | `/v1/projects/temporary_storage` | Temporary storage upload |
| `POST` | `/v1/projects/temporary_storage/uploads/{uploadId}` | Temporary storage part upload |
| `POST` | `/v1/projects/temporary_storage/complete` | Complete temporary storage upload |
| **Storage Transfers** | | |
| `POST` | `/v1/projects/{projectId}/storage-transfers` | Create transfer |
| `GET` | `/v1/projects/{projectId}/storage-transfers` | List transfers |
| `GET` | `/v1/projects/{projectId}/storage-transfers/{id}` | Get transfer |
| `POST` | `/v1/projects/{projectId}/storage-transfers/{id}/abort` | Abort transfer |
| `POST` | `/v1/projects/{projectId}/storage-transfers/{id}/pause` | Pause transfer |
| `POST` | `/v1/projects/{projectId}/storage-transfers/{id}/resume` | Resume transfer |
| `POST` | `/v1/projects/{projectId}/storage-transfers/validate` | Validate transfer |
| `GET` | `/v1/projects/{projectId}/cloudspaces/{cloudSpaceId}/artifacts/events` | Get artifact events |

---

## Dataset Service (Lit Datasets)

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/lit-datasets` | Create dataset |
| `GET` | `/v1/projects/{projectId}/lit-datasets` | List datasets |
| `GET` | `/v1/projects/{projectOwnerName}/{projectName}/lit-datasets/{datasetName}` | Get dataset by name |
| `PUT` | `/v1/projects/{projectId}/lit-datasets/{datasetId}` | Update dataset |
| `DELETE` | `/v1/projects/{projectId}/lit-datasets/{datasetId}` | Delete dataset |
| `PUT` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/visibility` | Set visibility |
| `POST` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions` | Create version |
| `GET` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions` | List versions |
| `GET` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}` | Get version |
| `PUT` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}` | Update version |
| `DELETE` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}` | Delete version |
| `PUT` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/default` | Set default version |
| `GET` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/files` | List version files |
| `POST` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads` | Start upload |
| `POST` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads/{uploadId}/parts` | Upload part |
| `POST` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/uploads/{uploadId}/complete` | Complete upload |
| `POST` | `/v1/projects/{projectId}/lit-datasets/{datasetId}/versions/{version}/complete` | Complete version |

---

## Models Store

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/models` | Create model |
| `GET` | `/v1/projects/{projectId}/models` | List models |
| `GET` | `/v1/models` | List public models |
| `GET` | `/v1/projects/{projectId}/models/{modelId}` | Get model |
| `GET` | `/v1/projects/{projectOwnerName}/{projectName}/models/{modelName}` | Get model by name |
| `PUT` | `/v1/projects/{projectId}/models/{modelId}` | Update model |
| `DELETE` | `/v1/projects/{projectId}/models/{modelId}` | Delete model |
| `PUT` | `/v1/projects/{projectId}/models/{modelId}/visibility` | Set visibility |
| `POST` | `/v1/projects/{projectId}/models/{modelId}/versions` | Create version |
| `GET` | `/v1/projects/{projectId}/models/{modelId}/versions` | List versions |
| `GET` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}` | Get version |
| `PUT` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}` | Update version |
| `DELETE` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}` | Delete version |
| `PUT` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/default` | Set default version |
| `GET` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/file` | Get version file |
| `GET` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/files` | List version files |
| `POST` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads` | Start upload |
| `POST` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads/{uploadId}/parts` | Upload part |
| `POST` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/uploads/{uploadId}/complete` | Complete upload |
| `POST` | `/v1/projects/{projectId}/models/{modelId}/versions/{version}/complete` | Complete version |

---

## Secret Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/secrets` | Create project secret |
| `GET` | `/v1/projects/{projectId}/secrets` | List project secrets |
| `GET` | `/v1/projects/{projectId}/secrets/{id}` | Get project secret |
| `PUT` | `/v1/projects/{projectId}/secrets/{id}` | Update project secret |
| `DELETE` | `/v1/projects/{projectId}/secrets/{id}` | Delete project secret |
| `POST` | `/v1/secrets` | Create user secret |
| `GET` | `/v1/secrets` | List user secrets |
| `GET` | `/v1/secrets/{id}` | Get user secret |
| `PUT` | `/v1/secrets/{id}` | Update user secret |
| `DELETE` | `/v1/secrets/{id}` | Delete user secret |

---

## SSH Public Key Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/ssh-keys` | Add SSH public key |
| `GET` | `/v1/ssh-keys` | List SSH keys |
| `GET` | `/v1/ssh-keys/{id}` | Get SSH key |
| `DELETE` | `/v1/ssh-keys/{id}` | Delete SSH key |
| `POST` | `/v1/ssh-keys/generate` | Generate SSH key pair |
| `GET` | `/v1/ssh-keys/{id}/confirm` | Confirm SSH key |

---

## Endpoint Service (Ports)

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/endpoints` | Create endpoint (expose port) |
| `GET` | `/v1/projects/{projectId}/endpoints` | List endpoints |
| `GET` | `/v1/projects/{projectId}/endpoints/{ref}` | Get endpoint |
| `PUT` | `/v1/projects/{projectId}/endpoints/{id}` | Update endpoint |
| `DELETE` | `/v1/projects/{projectId}/endpoints/{id}` | Delete endpoint |

---

## Cluster Service

Clusters are the underlying infrastructure pools. Most users use the default `lightning-cloud` cluster.

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/core/clusters` | Create cluster |
| `GET` | `/v1/core/clusters` | List clusters |
| `GET` | `/v1/core/clusters/{id}` | Get cluster |
| `PUT` | `/v1/core/clusters/{id}` | Update cluster |
| `DELETE` | `/v1/core/clusters/{id}` | Delete cluster |
| `GET` | `/v1/core/clusters/{id}/health` | Get cluster health |
| `GET` | `/v1/core/clusters/{id}/accelerators` | List cluster accelerators (machines) |
| `GET` | `/v1/core/accelerators` | List all accelerators |
| `GET` | `/v1/core/cluster-availability` | Get cluster availability |
| `GET` | `/v1/core/cluster-availabilities` | List availabilities |
| `PUT` | `/v1/core/cluster-availability` | Update availability |
| `GET` | `/v1/core/cluster-credentials` | Get cluster credentials |
| `POST` | `/v1/core/cluster-name-available` | Check name availability |
| `POST` | `/v1/core/cluster-encryption-keys` | Create encryption keys |
| `GET` | `/v1/core/cluster-encryption-keys` | List encryption keys |
| `DELETE` | `/v1/core/cluster-encryption-keys/{id}` | Delete encryption key |
| `PUT` | `/v1/core/clusters/accelerators` | Update accelerators |
| `GET` | `/v1/projects/{projectId}/clusters` | List project clusters |
| `POST` | `/v1/projects/{projectId}/clusters` | Add cluster to project |
| `GET` | `/v1/projects/{projectId}/clusters/{id}` | Get project cluster |
| `PUT` | `/v1/projects/{projectId}/clusters/{id}` | Update project cluster |
| `DELETE` | `/v1/projects/{projectId}/clusters/{id}` | Remove cluster from project |
| `GET` | `/v1/projects/{projectId}/clusters/{id}/accelerators` | List cluster accelerators |
| `PUT` | `/v1/projects/{projectId}/clusters/{id}/accelerators` | Update cluster accelerators |
| `POST` | `/v1/core/request-cluster-access` | Request cluster access |
| `POST` | `/v1/core/clusters/{clusterId}/machines` | Create machine |
| `GET` | `/v1/core/clusters/{clusterId}/machines` | List machines |
| `DELETE` | `/v1/core/clusters/{clusterId}/machines/{id}` | Delete machine |
| `POST` | `/v1/core/clusters/{clusterId}/proxies` | Create proxy |
| `GET` | `/v1/core/clusters/{clusterId}/proxies` | List proxies |
| `DELETE` | `/v1/core/clusters/{clusterId}/proxies/{proxyId}` | Delete proxy |
| `POST` | `/v1/core/clusters/{clusterId}/usage-restrictions` | Create usage restriction |
| `GET` | `/v1/core/clusters/{clusterId}/usage-restrictions` | List usage restrictions |
| `PUT` | `/v1/core/clusters/{clusterId}/usage-restrictions/{id}` | Update usage restriction |
| `DELETE` | `/v1/core/clusters/{clusterId}/usage-restrictions/{id}` | Delete usage restriction |
| `GET` | `/v1/core/clusters/{clusterId}/accelerator/{id}/demand` | Get accelerator demand |
| `POST` | `/v1/projects/{projectId}/clusters/{clusterId}/capacity-reservations` | Create capacity reservation |
| `GET` | `/v1/projects/{projectId}/clusters/{clusterId}/capacity-reservations/{id}` | Get capacity reservation |
| `DELETE` | `/v1/projects/{projectId}/clusters/{clusterId}/capacity-reservations/{id}` | Delete capacity reservation |
| `POST` | `/v1/projects/{projectId}/clusters/{clusterId}/capacity-block` | Create capacity block |
| `GET` | `/v1/projects/{projectId}/clusters/{clusterId}/capacity-block-offering` | Get capacity block offerings |

---

## Organizations Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/orgs` | Create organization |
| `GET` | `/v1/orgs` | List organizations |
| `GET` | `/v1/orgs:search` | Search organizations |
| `GET` | `/v1/orgs/{id}` | Get organization |
| `PUT` | `/v1/orgs/{id}` | Update organization |
| `DELETE` | `/v1/orgs/{id}` | Delete organization |
| `GET` | `/v1/orgs/joinable/list` | List joinable orgs |
| `POST` | `/v1/orgs/{orgId}/join` | Join organization |
| `POST` | `/v1/orgs/{orgId}/memberships` | Add member |
| `GET` | `/v1/orgs/{orgId}/memberships` | List members |
| `GET` | `/v1/orgs/{orgId}/members` | Get members |
| `DELETE` | `/v1/orgs/{orgId}/memberships/{userId}` | Remove member |
| `POST` | `/v1/orgs/{orgId}/memberships/{userId}/membershiprolebindings` | Add member role |
| `GET` | `/v1/orgs/{orgId}/memberships/{userId}/membershiprolebindings` | Get member roles |
| `DELETE` | `/v1/orgs/{orgId}/memberships/{userId}/membershiprolebindings/{roleId}` | Remove member role |
| `POST` | `/v1/orgs/{orgId}/roles` | Create role |
| `GET` | `/v1/orgs/{orgId}/roles` | List roles |
| `GET` | `/v1/orgs/{orgId}/roles/{id}` | Get role |
| `PUT` | `/v1/orgs/{orgId}/roles/{id}` | Update role |
| `DELETE` | `/v1/orgs/{orgId}/roles/{id}` | Delete role |
| `PUT` | `/v1/orgs/{orgId}/credits/auto-replenish` | Set credit auto-replenish |
| `POST` | `/v1/orgs/{orgId}/approveautojoindomain/{domain}` | Approve auto-join domain |
| `POST` | `/v1/orgs/{orgId}/validateautojoindomain/{domain}` | Validate auto-join domain |

---

## User Service

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/users/profile/{username}` | Get user profile |
| `GET` | `/v1/users/search` | Search users |
| `GET` | `/v1/users/settings/{filename}` | Get user settings file |
| `POST` | `/v1/users/settings/{filename}` | Save user settings file |
| `GET` | `/v1/users/complete-onboarding` | Complete onboarding |
| `GET` | `/v1/users/request-verification` | Request email verification |
| `GET` | `/v1/users/verify-verification` | Verify email |
| `GET` | `/v1/users/notification-dialogs` | Get notification dialogs |
| `PUT` | `/v1/users/new-features` | Acknowledge new features |
| `GET` | `/v1/users/storage/breakdown` | Get storage usage breakdown |
| `PUT` | `/v1/users/storage/ack-violation` | Acknowledge storage violation |
| `PUT` | `/v1/users/{userId}/credits/auto-replenish` | Set credit auto-replenish |
| `POST` | `/v1/users/{userId}/affiliate-links` | Create affiliate link |
| `GET` | `/v1/users/{userId}/affiliate-links` | List affiliate links |
| `GET` | `/v1/users/{userId}/affiliate-links/{id}` | Get affiliate link |
| `PUT` | `/v1/users/{userId}/affiliate-links/{id}` | Update affiliate link |
| `DELETE` | `/v1/users/{userId}/affiliate-links/{id}` | Delete affiliate link |

---

## Billing Service

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/billing/subscription` | Get subscription |
| `PUT` | `/v1/billing/subscription` | Update subscription |
| `GET` | `/v1/billing/subscription/quote` | Get subscription quote |
| `POST` | `/v1/billing/subscription/checkout` | Checkout subscription |
| `POST` | `/v1/billing/checkout` | Create checkout session |
| `POST` | `/v1/billing/portal-session` | Create billing portal session |
| `POST` | `/v1/billing/annual-upsell` | Create annual upsell |
| `GET` | `/v1/billing/annual-upsell` | Get annual upsell |
| `GET` | `/v1/billing/usage-details` | Get usage details |
| `GET` | `/v1/billing/usage-report` | Get usage report |
| `GET` | `/v1/billing/assistant-session` | Get assistant session billing |
| `GET` | `/v1/users/billing/balance` | Get user balance |
| `POST` | `/v1/users/billing/transfer` | Transfer user balance |
| `GET` | `/v1/projects/{projectId}/billing/balance` | Get project balance |
| `GET` | `/v1/projects/{projectId}/billing/compute` | Get compute usage |
| `POST` | `/v1/projects/{projectId}/billing/transfer` | Transfer project balance |
| `GET` | `/v1/orgs/{orgId}/billing/balance` | Get org balance |
| `POST` | `/v1/orgs/{orgId}/billing/checkout` | Org checkout |
| `POST` | `/v1/orgs/{orgId}/billing/transfer` | Transfer org balance |
| `POST` | `/v1/billing/{userId}/upgrade-trigger` | Trigger upgrade |

---

## Pipelines Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/pipelines` | Create pipeline |
| `GET` | `/v1/projects/{projectId}/pipelines` | List pipelines |
| `GET` | `/v1/projects/{projectId}/pipelines/{id}` | Get pipeline |
| `GET` | `/v1/projects/{projectId}/pipelines/{name}/getbyname` | Get pipeline by name |
| `POST` | `/v1/projects/{projectId}/pipelines/{id}` | Update/run pipeline |
| `PUT` | `/v1/projects/{projectId}/pipelines/{id}` | Update pipeline |
| `DELETE` | `/v1/projects/{projectId}/pipelines/{id}` | Delete pipeline |

---

## Agents (Assistants) Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/agents` | Create agent |
| `GET` | `/v1/agents` | List agents |
| `GET` | `/v1/agents/{id}` | Get agent |
| `PUT` | `/v1/projects/{projectId}/agents/{id}` | Update agent |
| `DELETE` | `/v1/projects/{projectId}/agents/{id}` | Delete agent |
| `GET` | `/v1/agents/{id}/knowledge` | Get agent knowledge base |
| `POST` | `/v1/agents/{id}/contact-owner` | Contact agent owner |
| `POST` | `/v1/agents/{id}/valid` | Validate agent |
| `GET` | `/v1/agents/latest-metrics` | Get agent metrics |
| `GET` | `/v1/agents/managed-model` | Get managed models |
| `GET` | `/v1/agents/managed-model/{modelName}` | Get managed model by name |
| `POST` | `/v1/agents/metrics/models/{modelId}` | Post model metrics |
| `GET` | `/v1/agents/metrics/models/{modelId}` | Get model metrics |
| **Conversations** | | |
| `POST` | `/v1/agents/{assistantId}/conversations` | Create conversation |
| `GET` | `/v1/agents/{assistantId}/conversations` | List conversations |
| `GET` | `/v1/agents/{assistantId}/conversations/{id}` | Get conversation |
| `PUT` | `/v1/agents/{assistantId}/conversations/{id}` | Update conversation |
| `DELETE` | `/v1/agents/{assistantId}/conversations/{id}` | Delete conversation |
| `PUT` | `/v1/projects/{projectId}/agents/{assistantId}/conversations/{id}` | Update conversation (project) |
| `PUT` | `/v1/projects/{projectId}/agents/{assistantId}/conversations/{conversationId}/messages/{id}/content` | Update message content |
| `PUT` | `/v1/projects/{projectId}/agents/{assistantId}/conversations/{conversationId}/messages/{id}` | Update message |
| `POST` | `/v1/agents/{assistantId}/conversations/{conversationId}/messages/{messageId}/actions` | Create message action |
| `GET` | `/v1/agents/{assistantId}/conversations/{conversationId}/messages/{messageId}/actions` | List message actions |
| **Managed Endpoints** | | |
| `POST` | `/v1/projects/{projectId}/agent-managed-endpoints` | Create managed endpoint |
| `GET` | `/v1/agent-managed-endpoints` | List managed endpoints |
| `PUT` | `/v1/projects/{projectId}/agent-managed-endpoints/{id}` | Update managed endpoint |
| `DELETE` | `/v1/projects/{projectId}/agent-managed-endpoints/{id}` | Delete managed endpoint |
| `GET` | `/v1/agent-published-managed-endpoints` | List published endpoints |
| `GET` | `/v1/agent-published-managed-model` | Get published managed model |
| `POST` | `/v1/agent-managed-endpoints/valid` | Validate managed endpoint |
| `PUT` | `/v1/projects/{projectId}/agent-managed-endpoints/{managedEndpointId}/models/{id}` | Update managed model |
| `GET` | `/v1/projects/{projectId}/agent-managed-endpoints/{managedEndpointId}/model/{name}` | Get managed model |
| `POST` | `/v1/projects/{projectId}/agent-managed-models/{id}/model/{modelId}/valid` | Validate managed model |

---

## Data Connection Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/data-connections` | Create data connection |
| `GET` | `/v1/projects/{projectId}/data-connections` | List data connections |
| `GET` | `/v1/projects/{projectId}/data-connections/{id}` | Get data connection |
| `PUT` | `/v1/projects/{projectId}/data-connections/{id}` | Update data connection |
| `DELETE` | `/v1/projects/{projectId}/data-connections/{id}` | Delete data connection |
| `GET` | `/v1/projects/{projectId}/data-connections/{id}/folder-index` | Get folder index |
| `GET` | `/v1/projects/{projectId}/data-connections/artifacts/{id}/page` | Get artifact page |
| `GET` | `/v1/projects/{projectId}/data-connections/artifacts/{id}` | Get artifact |
| `GET` | `/v1/projects/{projectId}/data-connections/{id}/temp-bucket-credentials` | Get temp credentials |
| `POST` | `/v1/projects/{projectId}/data-connections/{id}/index` | Re-index connection |
| `POST` | `/v1/projects/{projectId}/data-connections/setup` | Setup connection |
| `POST` | `/v1/projects/{projectId}/data-connections/valid` | Validate connection |

---

## K8S Cluster Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates` | Create K8s template |
| `GET` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates` | List K8s templates |
| `GET` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates/{id}` | Get K8s template |
| `PUT` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates/{id}` | Update K8s template |
| `DELETE` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates/{id}` | Delete K8s template |
| `POST` | `/v1/k8s-clusters/{clusterId}/kubernetes-templates/{id}/render` | Render template |
| `GET` | `/v1/k8s-clusters/{clusterId}/kubernetes-pods` | List pods |
| `GET` | `/v1/k8s-clusters/{clusterId}/kubernetes-pods/{id}` | Get pod |
| `GET` | `/v1/k8s-clusters/{clusterId}/kubernetes-pods/{id}/page-logs` | Get pod logs |
| `GET` | `/v1/k8s-clusters/{clusterId}/cluster-metrics` | Get cluster metrics |
| `POST` | `/v1/k8s-clusters/{clusterId}/metrics` | Post metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/metrics/nodes/{nodeName}` | Get node metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/metrics/pods/{podId}` | Get pod metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/metrics/pods/{podId}/containers/{containerId}` | Get container metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/metrics/group-pod` | Get group pod metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/aggregated-metrics/nodes/{nodeName}` | Get aggregated node metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/aggregated-metrics/pods` | Get aggregated pod metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/cluster-namespace-metrics` | Get namespace metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/cluster-namespace-user-metrics` | Get namespace user metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/cluster-metrics-timestamps` | Get metrics timestamps |
| `GET` | `/v1/projects/{projectId}/clusters/{clusterId}/cluster-metrics-timestamps` | Get project metrics timestamps |
| `GET` | `/v1/k8s-clusters/{clusterId}/cluster-kai-scheduler-queues-metrics` | Get scheduler queue metrics |
| `GET` | `/v1/k8s-clusters/{clusterId}/filesystem` | Get cluster filesystem |
| `GET` | `/v1/k8s-clusters/{clusterId}/metrics-filesystem/nodes/{nodeName}` | Get node filesystem metrics |

---

## Slurm Jobs Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/slurm/jobs` | Submit Slurm job |
| `GET` | `/v1/projects/{projectId}/slurm/jobs` | List Slurm jobs |
| `GET` | `/v1/projects/{projectId}/slurm/jobs/{id}` | Get Slurm job |
| `POST` | `/v1/projects/{projectId}/slurm/jobs/{id}` | Update Slurm job |
| `DELETE` | `/v1/projects/{projectId}/slurm/jobs/{id}` | Delete Slurm job |
| `GET` | `/v1/projects/{projectId}/slurm/jobs/{id}/logs` | Get Slurm job logs |
| `PUT` | `/v1/projects/{projectId}/slurm/jobs/{id}/action` | Perform action on job |
| `POST` | `/v1/core/clusters/{clusterId}/slurm-users` | Create Slurm user |
| `GET` | `/v1/core/clusters/{clusterId}/slurm-users` | List Slurm users |

---

## Git Credentials Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/git-credentials` | Add Git credentials |
| `GET` | `/v1/git-credentials` | List Git credentials |
| `GET` | `/v1/git-credentials/{id}` | Get Git credential |
| `DELETE` | `/v1/git-credentials/{id}` | Delete Git credential by ID |
| `DELETE` | `/v1/git-credentials` | Delete all Git credentials |

---

## Container Registry Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/core/clusters/{clusterId}/container-registries` | Create registry |
| `GET` | `/v1/core/clusters/{clusterId}/container-registries` | List registries |
| `PATCH` | `/v1/core/clusters/{clusterId}/container-registries/{id}` | Update registry |
| `DELETE` | `/v1/core/clusters/{clusterId}/container-registries/{id}` | Delete registry |
| `POST` | `/v1/core/clusters/{clusterId}/container-registries/refresh-credentials` | Refresh credentials |

---

## Virtual Machine Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/clusters/{clusterId}/virtual-machines` | Create VM |
| `GET` | `/v1/clusters/{clusterId}/virtual-machines` | List VMs |
| `GET` | `/v1/clusters/{clusterId}/virtual-machines/{id}` | Get VM |
| `PUT` | `/v1/clusters/{clusterId}/virtual-machines/{id}` | Update VM |
| `DELETE` | `/v1/clusters/{clusterId}/virtual-machines/{id}` | Delete VM |

---

## Volume Service

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/projects/{projectId}/volumes/{id}` | Get volume |
| `PUT` | `/v1/projects/{projectId}/volumes/{id}` | Update volume |

---

## Lit Registry Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/lit-registry` | Create registry |
| `GET` | `/v1/projects/{projectId}/lit-registry` | List registries |
| `GET` | `/v1/projects/{projectId}/lit-registry/{litRepoName}` | Get registry |
| `PUT` | `/v1/projects/{projectId}/lit-registry/{litRepoName}` | Update registry |
| `DELETE` | `/v1/projects/{projectId}/lit-registry/{litRepoName}` | Delete registry |
| `GET` | `/v1/projects/{projectId}/lit-registry/{litRepoName}/artifacts` | List artifacts |
| `GET` | `/v1/projects/{projectId}/lit-registry/{litRepoName}/artifacts/{fullHashDigest}` | Get artifact |
| `DELETE` | `/v1/projects/{projectId}/lit-registry/{litRepoName}/artifacts/{fullHashDigest}` | Delete artifact |

---

## Misc Services

### Deployment Templates Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/deployment-templates` | Create template |
| `GET` | `/v1/deployment-templates` | List templates |
| `GET` | `/v1/deployment-templates/published` | List published templates |
| `GET` | `/v1/deployment-templates/tags` | List tags |
| `GET` | `/v1/deployment-templates/{id}` | Get template |
| `PUT` | `/v1/deployment-templates/{id}` | Update template |
| `PUT` | `/v1/deployment-templates/{id}/engage` | Engage template |

### Analytics Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/analytics/onboarding` | Track onboarding event |

### Snowflake Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/projects/{projectId}/snowflake` | Configure Snowflake |
| `GET` | `/v1/projects/{projectId}/snowflake` | Get Snowflake config |
| `POST` | `/v1/projects/{projectId}/snowflake/query` | Execute query |
| `GET` | `/v1/projects/{projectId}/snowflake/query/{queryId}` | Get query result |
| `PUT` | `/v1/projects/{projectId}/snowflake/query/{queryId}` | Update query |
| `POST` | `/v1/projects/{projectId}/snowflake/export` | Export data |

### Product License Service

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/v1/licenses` | Create license |
| `GET` | `/v1/licenses` | List licenses |
| `GET` | `/v1/licenses/{id}` | Get license |
| `DELETE` | `/v1/licenses/{id}` | Delete license |
| `POST` | `/v1/licenses/{licenseKey}/validate` | Validate license |

### File System Service

| Method | Path | Description |
|--------|------|-------------|
| `GET` | `/v1/filesystem/{projectId}/cloudspaces/{currentId}` | Studio file system |
| `GET` | `/v1/filesystem/{projectId}/datasets/{currentId}` | Dataset file system |
| `GET` | `/v1/filesystem/{projectId}/jobs/{currentId}` | Job artifacts |
| `GET` | `/v1/filesystem/{projectId}/mmts/{currentId}` | MMT artifacts |
| `GET` | `/v1/filesystem/{projectId}/slurm/{currentId}` | Slurm job files |
| `GET` | `/v1/filesystem/{projectId}/snowflake/{currentId}` | Snowflake files |
| `GET` | `/v1/filesystem/{projectId}/volumes` | Volume files |
| `PUT` | `/v1/filesystem/{projectId}/volumes` | Update volumes |
| `GET` | `/v1/filesystem/trigger-upgrade` | Trigger filesystem upgrade |

---

## Summary

| Category | Endpoints |
|----------|-----------|
| CloudSpace (Studios) | 90 |
| Jobs & Deployments | 59 |
| Storage | 18 |
| Clusters | 35 |
| Organizations | 17 |
| Models Store | 17 |
| K8S Clusters | 24 |
| Agents/Assistants | 19 |
| Projects | 14 |
| Billing | 14 |
| Users | 14 |
| Lit Datasets | 15 |
| Lit Logger | 20 |
| Authentication | 12 |
| Lightningapp Instance | 13 |
| Secret | 8 |
| Data Connection | 12 |
| Other | ~147 |
| **Total** | **638** |
