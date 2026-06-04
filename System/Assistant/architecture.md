# System Architecture

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
