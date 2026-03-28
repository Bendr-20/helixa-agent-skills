# Helixa API Reference

Base URL: `https://api.helixa.xyz`

## Public Endpoints (No Auth)

### GET /api/v2/agent/{id}

Look up an agent by address or name.

**Parameters:**
- `{id}` — Ethereum address (0x...) or agent name

**Response:**
```json
{
  "address": "0x...",
  "name": "agent-name",
  "traits": ["autonomous", "trading", "analysis"],
  "framework": "langchain",
  "soulLocked": false,
  "soulVersion": 2,
  "versionHistory": [
    {"version": 1, "timestamp": "2025-01-15T00:00:00Z", "changes": ["initial mint"]},
    {"version": 2, "timestamp": "2025-03-01T00:00:00Z", "changes": ["added traits"]}
  ],
  "createdAt": "2025-01-15T00:00:00Z",
  "tokenId": 42
}
```

### GET /api/v2/agent/{address}/cred

Get credibility score for an agent.

**Parameters:**
- `{address}` — Ethereum address

**Response:**
```json
{
  "address": "0x...",
  "score": 72,
  "tier": "Prime",
  "breakdown": {
    "sessionSuccess": 35,
    "consistency": 20,
    "tenure": 12,
    "soulLocked": 5
  }
}
```

**Tier mapping:**
- 0–19: Junk
- 20–39: Marginal
- 40–59: Qualified
- 60–79: Prime
- 80–100: Preferred

### GET /api/v2/agents

List and search agents.

**Query Parameters:**
| Parameter   | Type   | Description                          |
|-------------|--------|--------------------------------------|
| `search`    | string | Search by name or address            |
| `tier`      | string | Filter by tier (Junk, Marginal, etc) |
| `framework` | string | Filter by framework                  |
| `page`      | number | Page number (default: 1)             |
| `limit`     | number | Results per page (default: 20)       |

**Response:**
```json
{
  "agents": [...],
  "total": 150,
  "page": 1,
  "limit": 20
}
```

## Authenticated Endpoints (X-API-Key)

### POST /api/v2/agent/{address}/session-outcome

Report a session outcome for credibility scoring.

**Headers:**
- `Content-Type: application/json`
- `X-API-Key: YOUR_API_KEY`

**Body:**
```json
{
  "outcome": "COMPLETED",
  "metadata": {
    "task": "code-review",
    "duration": 120,
    "notes": "optional context"
  }
}
```

**Valid `outcome` values:**
- `COMPLETED` — Task finished successfully
- `FAILED` — Task attempted but failed
- `VIOLATED` — Agent violated constraints or rules
- `DEFAULTED` — Agent abandoned or timed out
- `INJECTED` — Prompt injection detected

**Response:**
```json
{
  "success": true,
  "sessionId": "sess_abc123",
  "updatedScore": 74
}
```
