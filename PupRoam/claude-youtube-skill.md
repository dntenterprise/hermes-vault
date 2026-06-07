# claude-youtube — YouTube Automation Skill

**Källa:** https://github.com/AgriciDaniel/claude-youtube
**Importerad:** 2026-06-07
**Antal filer:** 39 (14 sub-skills + 9 references + 9 templates + 6 scripts + 1 SKILL.md)

## Översikt
Comprehensive YouTube automation skill for Hermes Agent. 14 commands för allt från channel audit till monetization planning. 5,300+ rader markdown, 1,300+ rader Python.

## Installationsväg
```bash
git clone https://github.com/AgriciDaniel/claude-youtube.git
```
Importerad som Hermes skill under `media/claude-youtube`.

## 14 Kommandon
- `/youtube audit` — Full channel health audit
- `/youtube seo` — Video SEO package
- `/youtube script` — Retention-engineered script
- `/youtube hook` — 5 hook variants
- `/youtube thumbnail` — Thumbnail brief med A/B varianter
- `/youtube strategy` — Channel positioning & milestones
- `/youtube calendar` — Monthly content calendar
- `/youtube shorts` — Shorts production package
- `/youtube analyze` — Analytics interpretation
- `/youtube repurpose` — Cross-platform repurposing
- `/youtube monetize` — Revenue strategy & rate cards
- `/youtube competitor` — Competitor analysis
- `/youtube metadata` — Upload package
- `/youtube ideate` — Video idea generation

## Filstruktur
```
skills/media/claude-youtube/
  SKILL.md                    # Orchestrator
  sub-skills/                 # 14 st
  references/                 # 9 st (algorithm, seo, retention, shorts, analytics, etc.)
  templates/                  # 9 st (education, entertainment, tutorial, vlog, etc.)
  execution/                  # 6 Python scripts (YouTube API, DataForSEO integration)
```

## Python-beroenden
- google-api-python-client (YouTube Data API v3)
- yt-dlp (transcript extraction)
- pandas (data analysis)

Kräver YouTube API key + OAuth 2.0 för full funktionalitet.
