# UniGen API — Context

## Concept Declaration

UniGen API — One unified endpoint for AI agents to generate images, video, and audio from any provider. No API keys. No accounts. Just USDC.

## Core Philosophy

UniGen is a **pipe**. The agent specifies the provider and model. UniGen passes the request through to the provider without modification. There is no routing optimization, no model recommendation, no smart selection. The agent decides everything; UniGen executes.

## Architecture

- **Static frontend**: `index.html`, `llms.txt`, `openapi.yaml`, `.well-known/ai-plugin.json`
- **Hosting**: Cloudflare Pages at `unigen-api.ai`
- **Backend**: Not yet implemented (API endpoints are spec-only at this stage)
- **Payment**: USDC via x402 protocol (machine-to-machine payments)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Hosting | Cloudflare Pages |
| Domain | unigen-api.ai |
| API Spec | OpenAPI 3.1 |
| Payment | USDC / x402 protocol |

## Endpoints (6)

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/v1/generate` | POST | Media generation (image, video, audio) |
| `/v1/estimate` | POST | Pre-generation estimate (price, time, success rate) |
| `/v1/capabilities` | GET | List supported providers and models |
| `/v1/pricing` | GET | Current pricing table |
| `/v1/status` | GET | Service operational status per provider |
| `/v1/balance` | GET | Check USDC balance for a wallet |

## Key Design Decisions

- All responses include a `suggestion` field — next action hints for the agent
- `parameters` in generate/estimate requests are passed through to providers as-is
- No authentication layer — payment via x402 replaces API keys
- Agent plugin manifest at `/.well-known/ai-plugin.json` for discovery

## Repository Structure

```text
unigen-api/
├── .ai/
│   ├── README.md
│   ├── CONTEXT.md      # This file
│   └── STATUS.md
├── .well-known/
│   └── ai-plugin.json  # Agent plugin discovery manifest
├── index.html           # Structured HTML landing page
├── llms.txt             # LLM crawler self-introduction
├── openapi.yaml         # OpenAPI 3.1 spec draft
├── LICENSE
└── README.md
```
