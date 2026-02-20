---
name: heurist-mesh-skill
description: Crypto and blockchain tools via Heurist Mesh REST API - token prices, DeFi analytics, wallet analysis, Twitter/X intelligence, web search.
---

# Heurist Mesh

Heurist Mesh is an open network of modular AI agent tools for crypto data, blockchain analytics, social intelligence, and web search. Each agent exposes tools callable via REST API.

### Recommended Agents and Tools

**Tool schemas with pricing:** First, you should use `GET /mesh_schema` to get detailed tool argument schemas for specific agents:
```
GET https://mesh.heurist.xyz/mesh_schema?agent_id=TokenResolverAgent&agent_id=CoinGeckoTokenInfoAgent
```
Add `&pricing=usd` to get prices in USD instead of credits. Returns each tool's parameters (name, type, description, required/optional) and per-tool price.

**TrendingTokenAgent** — Trending tokens and market summary
- `get_trending_tokens` — Get trending tokens most talked about and traded on CEXs and DEXs
- `get_market_summary` — Get recent market-wide news including macro and major updates

**TokenResolverAgent** — Find tokens and get detailed profiles
- `token_search` — Find tokens by address, ticker/symbol, or name (up to 5 candidates)
- `token_profile` — Get detailed token profile with pairs, funding rates, and indicators

**DefiLlamaAgent** — DeFi protocol and chain metrics
- `get_protocol_metrics` — Get protocol TVL, fees, volume, revenue, chains, and growth trend
- `get_chain_metrics` — Get blockchain TVL, fees, top protocols, and growth trends

**TwitterIntelligenceAgent** — Twitter/X data
- `user_timeline` — Fetch a user's recent posts and announcements
- `tweet_detail` — Get a tweet with thread context and replies
- `twitter_search` — Search for posts and influential mentions on any topic

**ExaSearchDigestAgent** — Web search with summarization
- `exa_web_search` — Search the web with LLM summarization, time and domain filters
- `exa_scrape_url` — Scrape a URL and summarize or extract information

**ChainbaseAddressLabelAgent** — EVM address labels
- `get_address_labels` — Get labels for ETH/Base addresses (identity, contract names, wallet behavior, ENS)

**ZerionWalletAnalysisAgent** — EVM wallet holdings
- `fetch_wallet_tokens` — Get token holdings with USD value and 24h price change
- `fetch_wallet_nfts` — Get NFT collections held by a wallet

**ProjectKnowledgeAgent** — Crypto project database
- `get_project` — Look up a project by name, symbol, or X handle (team, investors, events)
- `semantic_search_projects` — Natural language search across 10k+ projects (filter by investor, tag, funding year, exchange)

**AskHeuristAgent** — Crypto Q&A and deep analysis
- `ask_heurist` — Submit a crypto question (normal or deep analysis mode)
- `check_job_status` — Check status of a pending analysis job

**CaesarResearchAgent** — Academic research
- `caesar_research` — Submit a research query for in-depth analysis
- `get_research_result` — Retrieve research results by ID

## Setup (MUST complete before making any API calls)

You need at least one payment method configured. **DO NOT call any Mesh tool APIs until setup is verified.**

### Step 1: Choose a payment method

**Option A: Heurist API Key (Recommended — simplest)**

1. Get an API key via ONE of:
   - Purchase credits at https://heurist.ai/credits
   - Claim 100 free credits via tweet (see [references/heurist-api-key.md](references/heurist-api-key.md))
2. Store the key in `.env` in the project root:
   ```
   HEURIST_API_KEY=your-api-key-here
   ```
3. All API calls use `Authorization: Bearer $HEURIST_API_KEY`

**Option B: x402 On-Chain Payment (USDC on Base)**

1. You need a wallet private key with USDC balance on Base.
2. Store the key in `.env` in the project root:
   ```
   WALLET_PRIVATE_KEY=0x...your-private-key
   ```
3. See [references/x402-payment.md](references/x402-payment.md) for the payment flow using `cast` (Foundry).

**Option C: Inflow Payment Platform** *(Coming soon)*

See [references/inflow-payment.md](references/inflow-payment.md).

### Step 2: Verify setup

Check that credentials are configured before proceeding:

- **API Key path:** Read `.env` and confirm `HEURIST_API_KEY` is set and non-empty.
- **x402 path:** Read `.env` and confirm `WALLET_PRIVATE_KEY` is set, starts with `0x`, and is 66 characters.

**If neither is configured, STOP and ask the user to set up a payment method. Do not make API calls without valid credentials.**

### Step 3: Make API calls

Once verified, use the credentials in requests:

```bash
# With API key
curl -X POST https://mesh.heurist.xyz/mesh_request \
  -H "Authorization: Bearer $HEURIST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"agent_id": "TokenResolverAgent", "input": {"tool": "token_search", "tool_arguments": {"query": "ETH"}, "raw_data_only": true}}'

# With x402 — sign with cast (Foundry), no account or SDK needed
# See references/x402-payment.md for the full cast-based flow and helper script
```

## Discover More Agents

**All agents:** Fetch `https://mesh.heurist.ai/metadata.json` for the full registry. We have 30+ specialized crypto analytics agents covering use cases such as: reading address transaction history, reading transaction details from hash, tracing USDC on Base, detailed Coingecko data, Firecrawl scraping, GoPlus security screening, checking Twitter account influence via Moni, using SQL to query blockchain data, etc.

**x402-enabled agents only:** Fetch `https://mesh.heurist.xyz/x402/agents` for agents supporting on-chain USDC payment on Base.
