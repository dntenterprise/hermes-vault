# Mem0 — AI Memory Layer

**Installerad:** 2026-06-07
**Typ:** Cloud (gratis Hobby-plan)
**API:** api.mem0.ai

## Plan (Hobby — Free)
- 10,000 memories
- 1,000 retrieval API calls/month
- 1 project

## Setup
- **Credentials:** `/opt/data/profiles/sickan/secure/mem0-credentials.json`
- **CLI config:** `/opt/data/profiles/sickan/home/.mem0/config.json`
- **Script:** `/opt/data/profiles/sickan/scripts/mem0.sh`
- **Venv:** `/opt/crawler/.venv`

## Kommandon
```bash
./mem0.sh add "något att komma ihåg"
./mem0.sh search "vad sa vi om X"
./mem0.sh get
```

## Claima kontot
Konto skapat via Agent Mode. Boss måste claima:
```bash
mem0 init --email dnt.enterprise@icloud.com
```
(Görs från terminal på Mac)

## Funktioner
- Automatisk minnesextraktion från konversationer
- Entity linking
- Multi-signal retrieval (semantisk + BM25 + entity)
- Single-pass ADD-only (inga överskrivningar, ackumulerar bara)
