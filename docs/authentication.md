---
layout: default
title: Authentication
nav_order: 2
parent: Home
---

# Authentication

The Lightning AI REST API supports multiple authentication methods. All requests must include an `Authorization` header.

---

## Credentials

To authenticate, you need your **User ID** and **API Key**. Find these at:

1. Log in to [lightning.ai](https://lightning.ai)
2. Go to **Settings → API Keys**
3. Copy your **User ID** and create or copy an **API Key**

You can also set them as environment variables (used by the `lightning-sdk`):

```bash
export LIGHTNING_USER_ID="your-user-id"
export LIGHTNING_API_KEY="your-api-key"
```

---

## Method 1: API Key (Basic Auth) — Recommended

Encode `userId:apiKey` in Base64 and pass as a `Basic` Authorization header. This is the primary method used by the SDK.

```bash
# Construct the auth header
AUTH=$(echo -n "${LIGHTNING_USER_ID}:${LIGHTNING_API_KEY}" | base64)

curl -H "Authorization: Basic ${AUTH}" \
     -H "Content-Type: application/json" \
     https://lightning.ai/v1/projects
```

**Python example:**

```python
import base64
import requests

user_id = "your-user-id"
api_key = "your-api-key"

token = base64.b64encode(f"{user_id}:{api_key}".encode()).decode()
headers = {
    "Authorization": f"Basic {token}",
    "Content-Type": "application/json",
}

response = requests.get("https://lightning.ai/v1/projects", headers=headers)
print(response.json())
```

---

## Method 2: JWT Bearer Token

Exchange your API key for a short-lived JWT token. The token is valid for the specified duration (default: 12 hours).

### Login with API Key

**Endpoint:** `POST /v1/auth/login`

**Request body:**

```json
{
  "username": "your-email@example.com",
  "apiKey": "your-api-key",
  "duration": "43200"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `username` | string | No | Your Lightning AI account email |
| `apiKey` | string | No | Your API key |
| `duration` | string | No | Token validity in seconds (default: `43200` = 12h, max: `129600` = 36h, min: `900` = 15min) |

**Response:**

```json
{
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Example:**

```bash
TOKEN=$(curl -s -X POST https://lightning.ai/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "you@example.com",
    "apiKey": "your-api-key"
  }' | jq -r '.token')

curl -H "Authorization: Bearer ${TOKEN}" https://lightning.ai/v1/projects
```

---

### Token Login (Auth Token Key)

**Endpoint:** `POST /v1/auth/token-login`

Authenticate using a token key (different from the API key — this is a special auth token key generated in the UI).

**Request body:**

```json
{
  "tokenKey": "your-auth-token-key"
}
```

**Response:**

```json
{
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### Refresh Token

**Endpoint:** `POST /v1/auth/refresh`

Refresh a JWT token before it expires. Requires a valid Bearer token.

**Request body:**

```json
{
  "duration": "43200"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `duration` | string | New token duration in seconds (900–129600) |

**Response:**

```json
{
  "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Example:**

```bash
NEW_TOKEN=$(curl -s -X POST https://lightning.ai/v1/auth/refresh \
  -H "Authorization: Bearer ${OLD_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"duration": "43200"}' | jq -r '.token')
```

---

## Method 3: Environment Variables

The `lightning-sdk` automatically reads credentials from environment variables:

| Variable | Description |
|----------|-------------|
| `LIGHTNING_USER_ID` | Your user ID |
| `LIGHTNING_API_KEY` | Your API key |
| `LIGHTNING_AUTH_TOKEN` | A JWT auth token (takes precedence over API key) |
| `LIGHTNING_CLOUD_URL` | Override the base URL (default: `https://lightning.ai`) |

**Priority order (highest to lowest):**
1. `LIGHTNING_AUTH_TOKEN` → `Bearer {token}`
2. `LIGHTNING_USER_ID` + `LIGHTNING_API_KEY` → `Basic {base64(userId:apiKey)}`
3. Credentials stored on disk at `~/.lightning/credentials.json`
4. Browser-based OAuth flow (interactive only)

---

## Method 4: Guest Login

**Endpoint:** `POST /v1/auth/guest-login`

Create a temporary guest account for unauthenticated access. Useful for demos.

**Request body:** `{}` (empty object)

**Response:**

```json
{
  "user": {
    "id": "guest-user-id",
    "apiKey": "temporary-api-key"
  }
}
```

---

## Other Auth Endpoints

### Get Current User

**Endpoint:** `GET /v1/auth/user`

Returns the currently authenticated user's profile.

```bash
curl -H "Authorization: Basic ${AUTH}" https://lightning.ai/v1/auth/user
```

### Update Current User

**Endpoint:** `PUT /v1/auth/user`

Update the current user's profile.

### Reset API Key

**Endpoint:** `PUT /v1/auth/reset-api-key`

Generates a new API key, invalidating the current one.

### Logout

**Endpoint:** `POST /v1/auth/logout`

Invalidates the current session.

### Magic Link Login

**Endpoint:** `POST /v1/auth/magic-link-login`

Authenticate using a magic link (email-based passwordless login).

### Get Notification Preferences

**Endpoint:** `GET /v1/auth/user/notification-preferences`

### Update Notification Preferences

**Endpoint:** `PUT /v1/auth/user/notification-preferences`

---

## Credentials Storage

The SDK stores credentials in `~/.lightning/credentials.json`:

```json
{
  "user_id": "your-user-id",
  "api_key": "your-api-key",
  "auth_token": "your-jwt-token"
}
```

The path can be overridden with the `LIGHTNING_CREDENTIAL_PATH` environment variable.

---

## Error Responses

Authentication errors return HTTP `401`:

```json
{
  "code": 16,
  "message": "Authentication failed",
  "details": []
}
```

When using the `lightning-sdk`, a `ConnectionError` is raised with the message: _"Authentication failed. Please run `lightning login`."_
