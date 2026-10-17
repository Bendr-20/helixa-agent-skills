---
name: helixa
description: Query Helixa agent identities and Cred compatibility scores, register identity, and report session outcomes on Base. Use when working with AI agent identity, discovery, reputation, credibility scoring, or the Helixa registry.
license: MIT
metadata:
  author: helixa
  version: "1.1"
  homepage: https://helixa.xyz
---

# Helixa - Agent Identity and Discovery

Helixa provides onchain agent identity and discovery on Base. Cred is the trust layer and is surfaced through Helixa compatibility APIs and [cred.exchange](https://cred.exchange).

- **API Base:** `https://api.helixa.xyz`
- **Web:** [helixa.xyz](https://helixa.xyz)
- **V2 Contract:** `0x2e3B541C59D38b84E3Bc54e977200230A204Fe60` (Base mainnet)

## 1. Check an Agent's Cred Score

The profile and Cred routes require a numeric Helixa token ID. If you only have a name or wallet, resolve it first with `GET /api/v2/agents?search=...`.

```bash
curl https://api.helixa.xyz/api/v2/agent/1/cred
```

Representative fields include `tokenId`, `name`, `credScore`, `tier`, `tierLabel`, `evidenceCoverage`, and `scale`. Treat the returned scale as authoritative.

### Cred Tier Mapping

| Score Range | Tier |
|---|---|
| 0-25 | Junk |
| 26-50 | Marginal |
| 51-75 | Qualified |
| 76-90 | Prime |
| 91-100 | Preferred |

## 2. Look Up Agent Identity

```bash
curl https://api.helixa.xyz/api/v2/agent/1
```

`{id}` is a numeric Helixa token ID. The response includes identity, owner and agent addresses, metadata, traits, framework, verification, and Cred compatibility fields.

## 3. Search Agents

```bash
curl "https://api.helixa.xyz/api/v2/agents?search=claude&page=1&limit=20"
```

Search accepts names, addresses, and token IDs. Use it to resolve a wallet or name before calling a token-ID route.

## 4. Browse Agents

```bash
curl "https://api.helixa.xyz/api/v2/agents?page=1&limit=20"
curl "https://api.helixa.xyz/api/v2/agents?framework=langchain"
```

## 5. Report Session Outcomes

This endpoint is keyed by the agent wallet address, not token ID, and requires a platform API key. `sessionId` and an ISO-8601 `timestamp` are mandatory.

```bash
curl -X POST https://api.helixa.xyz/api/v2/agent/0xAGENT_ADDRESS/session-outcome \
  -H "Content-Type: application/json" \
  -H "X-API-Key: $HELIXA_API_KEY" \
  -d '{"outcome":"COMPLETED","sessionId":"sess_123","timestamp":"2026-10-17T15:27:00Z","metadata":{"task":"code-review"}}'
```

Valid outcomes: `COMPLETED`, `FAILED`, `VIOLATED`, `DEFAULTED`, `INJECTED`.

## 6. Mint Agent Identity

Mint through `POST /api/v2/mint` with SIWA and x402, or call HelixaV2 directly on Base. API registration is currently $1.00 USDC; read `GET /api/v2/pricing` before paying. The direct contract price is mutable; call `mintPrice()` immediately before `mint(address,string,string,bool)`.

See `reference/contract.md` for the current ABI and direct-mint example.

## Reference

- `reference/api.md` - API reference
- `reference/contract.md` - HelixaV2 contract reference
