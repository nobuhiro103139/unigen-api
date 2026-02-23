# PR Review: devin/1771879135-initial-structure

**Date**: 2026-02-23
**Reviewer**: Claude Code
**Verdict**: Request Changes (1 bug found)

---

## 1. Concept Declaration Consistency — PASS

The vision statement is identical across all files:
- `openapi.yaml` (info.description)
- `index.html` (meta description, JSON-LD, visible text)
- `llms.txt` (header quote)
- `ai-plugin.json` (description_for_human, description_for_model)
- `.ai/CONTEXT.md` (Concept Declaration section)

No human-oriented marketing copy detected. Target audience (AI agents) is consistently stated.

## 2. openapi.yaml Design — FAIL (1 bug)

### BUG: Double `/v1` prefix

```yaml
servers:
  - url: https://unigen-api.ai/v1   # includes /v1

paths:
  /v1/generate:                       # also includes /v1
```

OpenAPI 3.x resolves full URL as `servers.url + path`, producing:
`https://unigen-api.ai/v1/v1/generate` — this is incorrect.

**Fix**: Change `servers.url` to `https://unigen-api.ai` (remove `/v1`). This aligns with the path format used in index.html, llms.txt, and CONTEXT.md.

### Other checks — all pass:
- No routing optimization, model recommendation, or smart selection logic
- All 6 endpoints present: /v1/generate, /v1/estimate, /v1/capabilities, /v1/pricing, /v1/status, /v1/balance
- `suggestion` field present in all response schemas (including ErrorResponse)
- Parameter names (provider, model, media_type, prompt, parameters, wallet) are natural as MCP tool call arguments

## 3. index.html Structure — PASS

- Schema.org `SoftwareApplication` JSON-LD present with `audience.audienceType: "AI Agents"`
- Minimal design: monospace, black bg, table-only layout. No decoration bloat
- Crawler-friendly: semantic HTML (header/main/footer), meta robots, canonical URL

## 4. llms.txt Information Density — PASS

- Target: "AI agents. Not humans." — clear
- Functionality: all 6 endpoints listed
- Payment: "USDC via x402 protocol" — stated
- Pipe principle: "UniGen does not choose models for you" — stated
- No extraneous information

## 5. Unauthorized Additions — PASS

- No routing logic added
- No provider recommendation added
- No human-facing UI elements added
- `.ai/` directory is project management context, appropriate for the repo

## Summary

| Criterion | Result |
|-----------|--------|
| Concept declaration consistency | PASS |
| openapi.yaml design | **FAIL** — double `/v1` prefix |
| index.html structure | PASS |
| llms.txt density | PASS |
| Unauthorized additions | PASS |

**Required fix**: Change `openapi.yaml` line 18 `servers.url` from `https://unigen-api.ai/v1` to `https://unigen-api.ai`.

Once fixed → **approve**.
