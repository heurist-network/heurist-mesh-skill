# x402 On-Chain Payment

Pay per tool call with USDC on Base. No account needed — just a private key with USDC balance.

## Endpoint Pattern

```
POST https://mesh.heurist.xyz/x402/agents/{AgentId}/{toolName}
```

Example:
```
POST /x402/agents/AIXBTProjectInfoAgent/search_projects
```

## Payment Flow

1. **Call endpoint without payment** — returns `402 Payment Required` with payment metadata:
   ```json
   {
     "x402Version": 1,
     "accepts": [{
       "payTo": "0x7d9d...",
       "maxAmountRequired": "10000",
       "network": "base",
       "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
       "maxTimeoutSeconds": 120
     }]
   }
   ```
2. **Complete payment** via X402 protocol (gasless USDC transfer on Base).
3. **Retry with payment proof** in `X-Payment` header — returns tool result.

## Request Format

```bash
POST /x402/agents/AIXBTProjectInfoAgent/search_projects
Content-Type: application/json

{"limit": 10, "ticker": "BTC"}
```

Tool arguments go directly in the request body (no `tool`/`tool_arguments` wrapper).

## Using the X402 Client SDK

```typescript
import { X402Client } from '@coinbase/x402-client';

const client = new X402Client();
const result = await client.call({
  url: 'https://mesh.heurist.xyz/x402/agents/AIXBTProjectInfoAgent/search_projects',
  method: 'POST',
  body: { ticker: 'ETH', limit: 5 }
});
```

## List All x402 Agents

```bash
curl https://mesh.heurist.xyz/x402/agents
```

Returns agent list with tool names, paths, and per-tool USD prices.

## Requirements

- A private key controlling a wallet with USDC on Base
- USDC contract on Base: `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
