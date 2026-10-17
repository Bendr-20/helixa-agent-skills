# Helixa V2 Contract Reference

## Deployment

- **Contract:** `0x2e3B541C59D38b84E3Bc54e977200230A204Fe60`
- **Chain:** Base mainnet (chain ID 8453)
- **RPC:** `https://mainnet.base.org`
- **Explorer:** https://basescan.org/address/0x2e3B541C59D38b84E3Bc54e977200230A204Fe60

## Minting an Agent Identity

The direct mint price is mutable. Call `mintPrice()` immediately before submitting a mint; do not hard-code an ETH amount.

### Minimal ABI

```json
[
  {
    "inputs": [],
    "name": "mintPrice",
    "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }],
    "stateMutability": "view",
    "type": "function"
  },
  {
    "inputs": [
      { "internalType": "address", "name": "agentAddress", "type": "address" },
      { "internalType": "string", "name": "name", "type": "string" },
      { "internalType": "string", "name": "framework", "type": "string" },
      { "internalType": "bool", "name": "soulbound", "type": "bool" }
    ],
    "name": "mint",
    "outputs": [{ "internalType": "uint256", "name": "tokenId", "type": "uint256" }],
    "stateMutability": "payable",
    "type": "function"
  }
]
```

### ethers.js

```javascript
import { ethers } from "ethers";

const contract = new ethers.Contract(
  "0x2e3B541C59D38b84E3Bc54e977200230A204Fe60",
  [
    "function mintPrice() view returns (uint256)",
    "function mint(address agentAddress,string name,string framework,bool soulbound) payable returns (uint256 tokenId)"
  ],
  signer
);

const value = await contract.mintPrice();
const tx = await contract.mint(
  agentWalletAddress,
  "my-agent",
  "custom",
  false,
  { value }
);
await tx.wait();
```

### Foundry cast

```bash
PRICE=$(cast call 0x2e3B541C59D38b84E3Bc54e977200230A204Fe60 \
  "mintPrice()(uint256)" --rpc-url https://mainnet.base.org)

cast send 0x2e3B541C59D38b84E3Bc54e977200230A204Fe60 \
  "mint(address,string,string,bool)" \
  0xYOUR_AGENT_ADDRESS "my-agent" "custom" false \
  --value "$PRICE" \
  --rpc-url https://mainnet.base.org \
  --keystore /path/to/keystore.json
```

A soulbound mint is intentionally non-transferable. Confirm that choice before passing `true`.
