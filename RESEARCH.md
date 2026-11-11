# Research: Telegram forks & recommended features for E2EGram

Last updated: 2026-11-11

## Top Android Telegram forks (community landscape)

| Client | Focus | Notable features | Fit for E2EGram |
|--------|--------|------------------|-----------------|
| **Mercurygram** (our base) | FOSS / de-Google | UnifiedPush, no GMS, OSM maps, Tor plugin, local folders | Already base |
| **Forkgram** | Small UX fixes | Unlimited local pins, delete-for-all default, secret-chat timers, no contact sync by default | Steal UX polish |
| **Nekogram / NekoX / Nagram** | Power-user | Proxy before login, folders, UI density, icon packs | Optional later |
| **AyuGram** | Privacy / ghost | Ghost mode (no read/online/typing), deleted-message keep, edit history | High demand; ToS-sensitive |
| **OpenGram** | Privacy + filters | Disable remote deletion, content filters, fewer premium upsells | Aligns with E2E ethos |
| **entinyGram / NagramXF** | Feature mashups | Ghost + anti-delete + no ads + translation | Source of ideas only |
| **zkgram** (desktop) | Real E2E layer on cloud chats | Independent crypto over Telegram transport | Long-term inspiration for true E2E beyond secret chats |

**Takeaway:** Successful forks either (a) remove Google/telemetry, (b) add ghost/anti-delete, or (c) polish UX. E2EGram should stay in (a)+(true E2E) and not become another kitchen-sink Nekogram clone.

## Recommended feature roadmap for E2EGram

### P0 — Core identity

1. **Clear Chat mode switch** — E2E only vs Normal Telegram
2. **Classic ↔ Modern skin** — runtime theme, classic ≈ 2013 paper-plane aesthetic
3. **Force secret-chat open paths** — search, profile, notifications when E2E-only is on
4. **APP_ID / APP_HASH** documented + CI secrets

### P1 — Privacy differentiators

5. **Ghost mode toggles** (optional, off by default): suppress typing / online / read receipts
6. **Keep deleted messages locally** (OpenGram-style)
7. **Default: contacts sync off**
8. **Stronger secret-chat defaults**

### P2 — Offline / mesh (BitChat)

9. **BitChat integration** — see BITCHAT.md (MVP = companion app bridge)

### P3 — Nice-to-have

10. Unlimited local pins / archive pulldown
11. Content filters for channels
12. Proxy-before-login polish
13. Side-by-side applicationId

### Deprioritize

- Full Nekogram feature dump
- Closed-source blobs
- Claiming all chats are E2E without a real crypto layer

## Positioning

**E2EGram:** Mercurygram privacy base + hard E2E-only mode + optional classic skin + optional offline BitChat mesh.
