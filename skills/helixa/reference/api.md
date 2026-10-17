# Helixa API Reference

Base URL: `https://api.helixa.xyz`

## Public endpoints

### `GET /api/v2/agent/{tokenId}`

Returns a full Helixa profile for a numeric token ID. Wallet addresses and names are not accepted in this path. Resolve them first:

```bash
curl "https://api.helixa.xyz/api/v2/agents?search=0xWALLET"
```

Representative profile fields include `tokenId`, `agentAddress`, `owner`, `name`, `framework`, `mintedAt`, `verified`, `soulbound`, `credScore`, `metadata`, `personality`, `narrative`, and `traits`.

### `GET /api/v2/agent/{tokenId}/cred`

Returns the current Cred compatibility response for a numeric Helixa token ID.

```json
{
  "tokenId": 1,
  "name": "Bendr 2.0",
  "credScore": 65,
  "tier": "QUALIFIED",
  "tierLabel": "Qualified",
  "evidenceCoverage": { "score": 72, "label": "Good" },
  "scale": {
    "junk": "0-25",
    "marginal": "26-50",
    "qualified": "51-75",
    "prime": "76-90",
    "preferred": "91-100"
  }
}
```

The payload may contain additional evidence, Intuition, and report-discovery fields. Use `credScore` for the numeric score and keep Evidence Coverage separate.

### `GET /api/v2/agents`

List and search agents.

| Parameter | Type | Description |
|---|---|---|
| `search` | string | Search by name, address, or token ID |
| `framework` | string | Filter by framework |
| `page` | number | Page number |
| `limit` | number | Results per page |
| `sort` | string | Supported sort field |
| `order` | `asc` or `desc` | Sort direction |

## Authenticated endpoint

### `POST /api/v2/agent/{address}/session-outcome`

Reports an outcome for a 40-hex-character EVM agent wallet address.

Headers:

- `Content-Type: application/json`
- `X-API-Key: <platform key>`

```json
{
  "outcome": "COMPLETED",
  "sessionId": "sess_123",
  "timestamp": "2026-10-17T15:27:00Z",
  "metadata": {
    "task": "code-review"
  }
}
```

`sessionId` and ISO-8601 `timestamp` are required. Valid outcomes are `COMPLETED`, `FAILED`, `VIOLATED`, `DEFAULTED`, and `INJECTED`.
