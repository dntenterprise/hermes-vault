# Mem0 Cloud — Persistent Memory Layer

**Status:** ✅ Active (installed June 7, 2026)
**Plan:** Hobby (free) — 10,000 memories, 1,000 API calls/month

## Setup

1. **Pip install:** `pip install mem0ai` → version 2.0.4
2. **API key:** Generated from [app.mem0.ai](https://app.mem0.ai) — `m0-t8jeC5upMe92x5R94lCU7HLqkBTH9CSDw5RdDyFy`
3. **Config:** `/root/.mem0/config.json` — kopplad till `dnt.enterprise@icloud.com`
4. **Account claimed via website** — email kopplad, agent mode avaktiverat

## Scripts

- **Bash wrapper:** `/opt/data/profiles/sickan/scripts/mem0.sh` — `./mem0.sh add|search|get|delete`
- **Python wrapper:** `/opt/data/profiles/sickan/scripts/mem0.py` — `python3 scripts/mem0.py add|search|get|delete`

## API Endpoints (all tested)

| Method | Endpoint | Body/Params | Returns |
|--------|----------|-------------|---------|
| GET | `/v1/memories/` | `?user_id=dnt.enterprise@icloud.com` | All memories |
| POST | `/v1/memories/` | `{"messages":[{"role":"user","content":"..."}], "user_id":"..."}` | PENDING + event_id |
| POST | `/v1/memories/search/` | `{"query":"...", "user_id":"..."}` | Scored results |
| DELETE | `/v1/memories/{id}/` | `?user_id=dnt.enterprise@icloud.com` | Deleted |

**Auth:** `Authorization: Token <api_key>`
**Base:** `https://api.mem0.ai/v1`

## Current Memories (7 added)

1. Sickan system-wide coordinator + 4 agents
2. Boss = Dino Valentino, Stockholm, decision-first, 24h, $0 mindset
3. PupRoam brand details + CJ dropshipping
4. Obsidian vault setup
5. 43 marketing skills
6. Vibiz MCP connection
7. CJ API setup

## Viktiga noteringar

- **mem0-funktioner** kräver en embedder (OpenAI som default). Vi använder **REST API direkt** för att slippa OpenAI-nyckel.
- **Mem0 lokal klient** fungerar inte utan OpenAI API-nyckel — använd alltid REST API eller wrappers.
- **Async processing:** add returnerar PENDING + event_id, memories syns efter några sekunder.
- **Auto-extraction:** Mem0 extraherar individuella fakta från message (t.ex. "Sickan är coordinator på Telegram" + "Boss är Dino Valentino" blir separata minnen från en add).
- **Ingen manuell save krävs** — anropa `m.add()` i scripts.
