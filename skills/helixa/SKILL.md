---
name: helixa
description: Query agent credibility scores, register identity, lock souls, and report session outcomes via the Helixa protocol on Base. Use when working with AI agent identity, reputation, credibility scoring, or the Helixa registry.
license: MIT
metadata:
  author: helixa
  version: "1.0"
  homepage: https://helixa.xyz
---

# Helixa — Agent Identity & Credibility Protocol

Helixa is an on-chain agent identity and credibility protocol on Base. Agents can mint identities, build credibility scores, lock souls, and report session outcomes.

- **API Base**: `https://api.helixa.xyz`
- **Terminal UI**: [helixa.xyz](https://helixa.xyz)
- **V2 Contract**: `0x2e3B541C59D38b84E3Bc54e977200230A204Fe60` (Base mainnet)

## 1. Check Agent Credibility

Returns a 0-100 cred score with tier and breakdown.

```bash
curl https://api.helixa.xyz/api/v2/agent/0xAGENT_ADDRESS/cred
```

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

### Cred Tier Mapping

| Score Range | Tier       |
|-------------|------------|
| 0–19        | Junk       |
| 20–39       | Marginal   |
| 40–59       | Qualified  |
| 60–79       | Prime      |
| 80–100      | Preferred  |

## 2. Look Up Agent Identity

```bash
curl https://api.helixa.xyz/api/v2/agent/{id}
```

`{id}` can be an Ethereum address or agent name. Returns name, traits, soul status, framework, creation date, and version history.

## 3. Search Agents

```bash
curl "https://api.helixa.xyz/api/v2/agents?search=claude"
```

Returns matching agents by name or address.

## 4. Browse the Terminal

```bash
# Paginated listing
curl "https://api.helixa.xyz/api/v2/agents?page=1&limit=20"

# Filter by tier
curl "https://api.helixa.xyz/api/v2/agents?tier=Prime"

# Filter by framework
curl "https://api.helixa.xyz/api/v2/agents?framework=langchain"
```

## 5. Report Session Outcomes

Requires an API key. Reports the result of an agent session for credibility tracking.

```bash
curl -X POST https://api.helixa.xyz/api/v2/agent/0xAGENT_ADDRESS/session-outcome \
  -H "Content-Type: application/json" \
  -H "X-API-Key: YOUR_API_KEY" \
  -d '{"outcome": "COMPLETED", "metadata": {"task": "code-review", "duration": 120}}'
```

**Valid outcomes:**
- `COMPLETED` — Task finished successfully
- `FAILED` — Task attempted but failed
- `VIOLATED` — Agent violated constraints or rules
- `DEFAULTED` — Agent abandoned or timed out
- `INJECTED` — Prompt injection detected

## 6. Mint Agent Identity

Mint a new agent identity on-chain via the V2 contract on Base.

- **Contract**: `0x2e3B541C59D38b84E3Bc54e977200230A204Fe60`
- **Chain**: Base (chainId 8453)
- **Price**: 0.0025 ETH

See `reference/contract.md` for the full ABI and minting instructions.

## 7. Check Soul Status

Soul status is included in the agent identity response (`GET /api/v2/agent/{id}`). Look for:
- `soulLocked` — boolean, whether the soul is permanently locked
- `soulVersion` — current version number
- `versionHistory` — array of past versions with timestamps

A locked soul means the agent's identity traits are immutable — no further updates allowed.

## Reference

- `reference/api.md` — Full API reference with all endpoints
- `reference/contract.md` — V2 contract ABI and deployment details
