# System Architecture

## Core Principle
The system uses a three-tier memory architecture to keep AI context windows lean while preserving accumulated knowledge over time.

## Data Flow

```
[External Sources] ──► [AI Assistant] ──► [Hot Memory / Vault]
     │                      │                      │
  Calendar               Your Tools           MEMORY.md (hot)
  Email                  (API, CLI)           context.md
  Tasks                  (browser, etc.)      preferences.md
  Weather                                    environment.md
     │                      │                      │
     └─────────► [Cron Jobs] ────► [Daily Notes] ──┘
```

## How It Works

1. **External data** comes in through tools and APIs
2. **The assistant** processes it and decides where it belongs
3. **Hot memory** gets immediate context — preferences, corrections, active state
4. **Vault files** get persistent storage — daily notes, operational docs, stable reference
5. **Vault feeds back** — the assistant reads vault files on-demand when it needs deeper context

## Why This Works

- **Plain text markdown** — the AI reads and writes without special plugins
- **Backlinks** create a visual graph of connections over time
- **Every daily note** is a timestamped record — searchable by you and the AI
- **The vault is still fully functional** even without the AI — no vendor lock-in
- **Three-tier memory** keeps context windows lean while preserving accumulated knowledge

## Components

### External Sources
- **Calendar** — time-blocking, deadlines, meetings
- **Email** — inbox zero, action items
- **Tasks** — TODO list, priorities
- **Weather** — Stockholm forecast (UTC+2)

### AI Assistant (Sickan)
- Orchestrates sub-agents (Scout, Scribe, Reach, Dev)
- Uses tools: API, CLI, browser, terminal
- Cron scheduling for recurring tasks
- Delegates, never solo-executes

### Hot Memory / Vault
| File | Purpose |
|------|---------|
| `MEMORY.md` | Hot procedural knowledge, tool quirks, lessons learned |
| `context.md` | Operations overview, health, family |
| `preferences.md` | Communication style, delivery rules |
| `environment.md` | Hardware, services, known issues |
| `USER.md` | User profile and preferences |
| `Daily/*.md` | Day-to-day log, decisions, tasks |

## Refresh Cadence
- **Hot memory**: updated every session (preferences, corrections, active state)
- **Vault files**: updated daily or on-demand
- **Daily notes**: every day via cron or manual entry
- **Backlinks**: visualized in Obsidian graph view

## Cron Pipeline
1. Cron triggers → runs script/agent
2. Agent polls external sources
3. Writes to Daily Notes
4. Feeds back into context/preferences

## Timezone
- **CEST (UTC+2)** — Sweden summer time
- **24h format** always (00:00-23:59)

## Agents
| Agent | Role |
|-------|------|
| 🕵️ **Scout** | Research, market data, competitors |
| ✍️ **Scribe** | Writing, blogs, newsletters |
| 📈 **Reach** | Marketing strategy, campaigns |
| 💻 **Dev** | Code, APIs, React, automation |
| **Sickan** | Orchestrator — system-wide coordinator |
