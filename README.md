# Hermes Vault — LLM Wiki

Persistent, compounding knowledge base. Synkat mellan VPS och Mac via GitHub.

## Arkitektur (Karpathy LLM Wiki pattern)

```
Hermes Vault/
├── AGENTS.md          # Schema — definierar struktur, konventioner, workflows (Layer 3)
├── raw/               # Rådata — immutable sources (Layer 1)
│   ├── articles/      #   Web articles, blog posts
│   ├── tiktok/        #   TikTok transcripts/notes
│   ├── docs/          #   PDFs, documents
│   └── assets/        #   Images, attachments
├── wiki/              # LLM-maintained markdown wiki (Layer 2)
│   ├── index.md       #   Content catalog
│   ├── log.md         #   Append-only chronological record
│   ├── entities/      #   People, companies, products
│   ├── concepts/      #   Ideas, theories, strategies
│   ├── sources/       #   Source summaries
│   ├── system/        #   System documentation
│   └── daily/         #   Daily notes
```

## Regler
- `raw/` = append-only — radera aldrig
- `wiki/` = LLM äger — skapar, uppdaterar, cross-refererar
- `AGENTS.md` = gemensam — du och LLM co-evolverar
- Varje ny källa → ingest workflow → uppdatera index + log
- Wiki-länkar: `[[wiki/entities/Name]]`
