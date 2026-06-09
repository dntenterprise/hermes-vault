# Brevo Email Setup — puproam.store

**API-nyckel:** Sparad i `/opt/data/profiles/sickan/brevo-credentials.json`

## Kontoinfo
- Konto: zn6gg2zs7n@privaterelay.appleid.com (Apple Hide My Email)
- Plan: Free (300 emails/day)
- Domän: puproam.store (verifierad + autentiserad)

## SMTP
- Server: smtp-relay.brevo.com:587
- Login: adc68f001@smtp-brevo.com
- Lösenord: Se Brevo dashboard → Transactional → SMTP

## Avsändare (verifierade)
- info@puproam.store (id: 2) — "PupRoam"
- hello@puproam.store (id: 3) — "PupRoam"

## Skicka mail från hello@puproam.store

### Via API (rekommenderat):
```bash
curl -s -X POST "https://api.brevo.com/v3/smtp/email" \
  -H "api-key: DIN_NYCKEL" \
  -H "Content-Type: application/json" \
  -d '{
    "sender": {"email": "hello@puproam.store", "name": "PupRoam"},
    "to": [{"email": "mottagare@example.com"}],
    "subject": "Ämne",
    "textContent": "Ditt meddelande"
  }'
```

### Via SMTP:
Server: smtp-relay.brevo.com:587
Login: adc68f001@smtp-brevo.com
Lösenord: Från dashboard
