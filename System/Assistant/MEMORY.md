# MEMORY.md — Assistant's Personal Notes

Accumulated procedural knowledge, environment facts, tool quirks, lessons learned.

§
DISCORD_SETUP: Sickan profile .env has Discord token (verified). Server=1511774680451776522, home channel=1511778973015871699, bot=1511689428207276042#5513. Channel bindings: #scout-briefs (1511803837797765165) → Scout, #scribe-scripts (1511803839727271976) → Scribe, #reach-marketing (1511803841232769025) → Reach, #dev-build (1511803842587525342) → Dev. Each agent has own channel_prompt.

§
GATEWAY_WATCHDOG: Cron @reboot + per-minute pgrep script at /opt/data/profiles/sickan/run-gateway.sh. Vixie cron, no systemd (PID1=ttyd). HERMES_BIN=/opt/hermes/.venv/bin/hermes.

§
LOGGING_RULE: Silently log every response via ~/.hermes/agents/_shared/log-task-local.sh before sending. Args: "<agent-name>" "<description under 140 chars>" "completed|failed". Agent names: scout, scribe, reach, dev. Do not mention logging to Boss unless asked.

§
CONTENT_PROTOCOL: All long-form docs (>~15 lines) go to /root/.hermes/content/{agent}/YYYY-MM-DD_kebab-title.md. Each agent writes only to its own subfolder. Markdown only. One file per deliverable. No silent overwrites. Never return long-form work inline.

§
TAILSCALE: Userspace-networking (no TUN). IP 100.113.138.61, MagicDNS https://62d2769f23cf.taileece85.ts.net. Dashboard (127.0.0.1:51763) via `tailscale serve --bg 51763`. No public tunnel. tailscaled runs as bg process — needs manual restart on reboot.

§
OBSIDIAN_VAULT: Local=/Users/dinovalentino/Desktop/Hermes vault, VPS=/opt/data/profiles/sickan/home/Obsidian/Hermes, repo=dntenterprise/hermes-vault. OBSIDIAN_VAULT_PATH in sickan profile .env. Auto-sync every 10 min via Obsidian Git plugin.

§
PIPELINE_RULE: ALL tasks delegated to right agent (Scout=research, Scribe=writing, Reach=marketing, Dev=code). Sickan orchestrates only — system coordination, cron, architecture, delegation. Never solo-execute tasks. Save workflows as skills.

§
TIMEZONE: CEST (UTC+2). Always 24h format (00:00-23:59). Never AM/PM.

§
AGENT_SERVER: Dashboard på port 51763 (Tailscale: https://62d2769f23cf.taileece85.ts.net). Server path: /root/agent-mission-control/server.py. Index: /root/agent-mission-control/index.html. Logs DB: agent-logs.db med SSE.

§
CRON_LEADS: Daily lead generation kl 06:00 UTC (08:00 CEST) via /root/agent-mission-control/stockholm_leads_daily.sh. Targets small businesses in Stockholm without good websites. Delivers 25 leads/day.

---
*Migration trigger: When this file hits ~4000 chars, promote stable entries to vault living files.*
