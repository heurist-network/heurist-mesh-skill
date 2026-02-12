---
name: heurist-mesh
description: Heurist Mesh provides AI agent tools for crypto and blockchain tasks via REST API. Use this skill when the user wants to (1) look up token prices, market data, or trending coins, (2) analyze a crypto project, wallet, or DeFi protocol, (3) search or read tweets/X posts about crypto, (4) search the web or research crypto topics, (5) get address labels or on-chain data, (6) discover Mesh agents/tools, call Mesh API endpoints, or understand payment options (API key credits, x402, Inflow). Triggers on: token price, market cap, trending tokens, wallet analysis, DeFi TVL, project info, Twitter/X search, web search, crypto research, Heurist, Mesh, and any blockchain/crypto data queries.
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

## Call a Tool

**Base URL:** `https://mesh.heurist.xyz`

Example: 
```
POST https://mesh.heurist.xyz/mesh_request
Authorization: Bearer <api_key>
Content-Type: application/json

{
  "agent_id": "TokenResolverAgent",
  "input": {
    "tool": "resolve_token",
    "tool_arguments": {"symbol": "ETH"},
    "raw_data_only": true
  }
}
```

## Advanced: Discover More Agents

**All agents:** Fetch `https://mesh.heurist.ai/metadata.json` for the full registry. We have 30+ specialized crypto analytics agents covering use cases such as: reading address transaction history, reading transaction details from hash, tracing USDC on Base, detailed Coingecko data, Firecrawl scraping, GoPlus security screening, checking Twitter account influence via Moni, using SQL to query blockchain data, etc.

**x402-enabled agents only:** Fetch `https://mesh.heurist.xyz/x402/agents` for agents supporting on-chain USDC payment on Base.

## Payment Methods

Three ways to pay for Mesh tool calls:

### 1. Heurist API Key (credits)
Prepaid credits deducted per tool call. Simplest option for getting started.
See [references/heurist-api-key.md](references/heurist-api-key.md).

### 2. x402 On-Chain Payment (USDC on Base)
Pay-per-call with crypto. No account needed — just a wallet with USDC on Base.
See [references/x402-payment.md](references/x402-payment.md).

### 3. Inflow Payment Platform
Agentic payment platform with user registration and approval flows.
See [references/inflow-payment.md](references/inflow-payment.md). *(Coming soon)*
