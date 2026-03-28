# Helixa V2 Contract Reference

## Deployment

- **Contract**: `0x2e3B541C59D38b84E3Bc54e977200230A204Fe60`
- **Chain**: Base (chainId 8453)
- **RPC**: `https://mainnet.base.org`
- **Explorer**: `https://basescan.org/address/0x2e3B541C59D38b84E3Bc54e977200230A204Fe60`

## Minting an Agent Identity

**Price**: 0.0025 ETH

### Mint Function ABI

```json
[
  {
    "inputs": [
      { "internalType": "string", "name": "name", "type": "string" },
      { "internalType": "string", "name": "metadataURI", "type": "string" }
    ],
    "name": "mint",
    "outputs": [
      { "internalType": "uint256", "name": "tokenId", "type": "uint256" }
    ],
    "stateMutability": "payable",
    "type": "function"
  }
]
```

### Minting with ethers.js

```javascript
import { ethers } from "ethers";

const CONTRACT = "0x2e3B541C59D38b84E3Bc54e977200230A204Fe60";
const ABI = [
  "function mint(string name, string metadataURI) payable returns (uint256 tokenId)"
];
const MINT_PRICE = ethers.parseEther("0.0025");

const provider = new ethers.JsonRpcProvider("https://mainnet.base.org");
const signer = new ethers.Wallet(PRIVATE_KEY, provider);
const contract = new ethers.Contract(CONTRACT, ABI, signer);

const tx = await contract.mint("my-agent", "ipfs://Qm.../metadata.json", {
  value: MINT_PRICE
});
const receipt = await tx.wait();
const tokenId = receipt.logs[0].args.tokenId;
console.log("Minted token:", tokenId.toString());
```

### Minting with cast (Foundry)

```bash
cast send 0x2e3B541C59D38b84E3Bc54e977200230A204Fe60 \
  "mint(string,string)" \
  "my-agent" "ipfs://Qm.../metadata.json" \
  --value 0.0025ether \
  --rpc-url https://mainnet.base.org \
  --private-key $PRIVATE_KEY
```

### Metadata Format

The `metadataURI` should point to a JSON file (typically on IPFS) with:

```json
{
  "name": "my-agent",
  "description": "An autonomous trading agent",
  "traits": ["trading", "defi", "autonomous"],
  "framework": "langchain",
  "image": "ipfs://Qm.../avatar.png"
}
```

## Other Contract Functions

### Read Functions

- `ownerOf(uint256 tokenId) → address` — Get owner of agent token
- `tokenURI(uint256 tokenId) → string` — Get metadata URI
- `mintPrice() → uint256` — Current mint price

### Write Functions

- `mint(string name, string metadataURI) payable → uint256` — Mint new agent identity
- `lockSoul(uint256 tokenId)` — Permanently lock agent soul (irreversible)
- `updateMetadata(uint256 tokenId, string metadataURI)` — Update metadata (only if soul not locked)
