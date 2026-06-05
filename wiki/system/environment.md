---
tags: [system, assistant, environment]
---

# Environment

## Hardware
- **VPS**: Linux 6.8.0, no systemd (PID 1 = ttyd)
- **Local**: Mac (Dino Valentino)

## Services & Ports
- **Mission Control**: 127.0.0.1:51763 → Tailscale serve
- **Tailscale**: 100.113.138.61, userspace-networking (no TUN)
- **Tailscale DNS**: https://62d2769f23cf.taileece85.ts.net

## Key Paths
- **Hermes profile**: `/opt/data/profiles/sickan/`
- **Agent logs DB**: `~/.hermes/agent-logs.db`
- **Content docs**: `/root/.hermes/content/{agent}/`
- **Skills**: `/opt/data/profiles/sickan/skills/`
- **Cron scripts**: `/opt/data/profiles/sickan/scripts/`

## Known Issues
- Tailscale needs manual restart on reboot (no systemd)
- Gateway auto-restart via cron @reboot + watchdog
- Port 51763 must be freed before restarting server
