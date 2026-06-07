# crawl4ai — Web Scraper (Firecrawl Replacement)

Installerad 2026-06-07 som ersättning för Firecrawl (Docker funkar inte i VPS-miljön).

## Setup
- **Venv:** `/opt/crawler/.venv`
- **Script:** `/opt/data/profiles/sickan/scripts/crawl.sh`
- **Aktivera:** `source /opt/crawler/.venv/bin/activate`
- **Browser:** Playwright Chromium (headless) i ~/.cache/ms-playwright/

## Användning
```bash
# Scrapa en webbsida (JS-rendering stöds)
./crawl.sh scrape <url>

# Returnerar JSON med markdown-content
```

**Exempel:** Scrapade puproam.store → 9,591 tecken markdown på 1.5s.

## Varför inte Firecrawl?
VPS:en är själv en container utan nödvändiga kernel-capabilities:
- ❌ `CAP_SYS_ADMIN` (saknas = ingen unshare)
- ❌ `overlay` kernel module (saknas)
- ❌ `iptables` (saknas = inget Docker-nätverk)

Docker fungerar inte → crawl4ai är det bästa Python-alternativet.
