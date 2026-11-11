# HANDOFF — E2EGram

**For any AI agent (Gemini, Claude, Grok, Cursor, Google AI Studio, etc.):**  
Read this file fully before changing code. Treat it as ground truth. Do not re-scaffold the project from scratch.

| Field | Value |
|-------|--------|
| **Repo** | https://github.com/imanbarati/E2EGram |
| **Owner** | imanbarati |
| **Branch** | `main` |
| **Base project** | [Mercurygram](https://github.com/Mercurygram/Mercurygram) (branch `Mercurygram`) — FOSS Telegram Android fork |
| **Last handoff update** | 2026-11-11 |

---

## Mission

Privacy-first client that:

1. Improves Telegram (Mercurygram) with **hard E2E-only mode** and **classic ↔ modern skin**
2. Can install **side-by-side** with official Telegram
3. Bridges to **BitChat** (offline mesh), and later **Matrix**, **Fediverse** (Pleroma/Soapbox), independent location search, and OpenAI-compatible AI assistants
4. Does **not** pretend multi-protocol is a few patches on MTProto

See [RESEARCH.md](RESEARCH.md) for full landscape and prioritization.

### Runtime switches (Telegram backend)

| Control | Options |
|---------|---------|
| **Chat mode** | E2E only (secret chats; hide cloud; block groups/channels) ↔ Normal Telegram |
| **Skin** | Classic (early Telegram aesthetic) ↔ Modern (current Material) |

Independent: any combination of mode × skin is valid.

---

## Current state

### Done

- [x] Repo + CI: clone Mercurygram → apply `*.java.patch` → arm64 debug APK + pre-release
- [x] `e2eOnly` in `MgAccountConfig` (save/load/reset)
- [x] Settings labels: “Chat mode: E2E only” / “Chat mode: Normal Telegram”
- [x] When E2E-only: filter dialogs, force secret on new write, block group/channel create
- [x] `useClassicSkin` flag in `MgAccountConfig` + settings row (theme application **not** done)
- [x] RESEARCH.md, BITCHAT.md, this HANDOFF
- [x] APP_ID / APP_HASH documented as GitHub Actions secrets

### Not done (P0–P2)

- [ ] Confirmation dialog on first enable of E2E-only
- [ ] Harden open paths (search/profile/notifications) to force secret chat when E2E-only
- [ ] Classic **theme** application (Theme / DialogCell / intro) — flag only today
- [ ] Distinct `applicationId` for side-by-side install
- [ ] String resources instead of hard-coded English
- [ ] BitChat MVP: settings section + drawer/intent to open BitChat app ([BITCHAT.md](BITCHAT.md))
- [ ] Smoke-test login with real APP_ID/APP_HASH

### Explicitly later (P3–P4 — architecture change)

- Independent location search (Nominatim / web) into compose
- In-app OpenAI-compatible AI assistant (separate activity)
- Matrix intent bridge → optional SDK account type
- Fediverse (Pleroma/Soapbox/Mastodon API) intent or account type
- Unified multi-protocol inbox

---

## Architecture notes

### Build model (do not break)

1. CI checks out `Mercurygram/Mercurygram` @ `Mercurygram`
2. Applies root `*.java.patch` with `patch -p1`
3. Builds `assembleAfatFdArm64Debug`

**Rule:** Prefer small reversible patches. Do not vendor the full Telegram tree unless the user explicitly decides to leave the patch model.

### Key patches

| Patch | Purpose |
|-------|---------|
| `MgAccountConfig.java.patch` | `e2eOnly`, `useClassicSkin` |
| `MercurygramSettingsActivity.java.patch` | Settings rows + handlers |
| `DialogsActivity.java.patch` | Force secret on new write |
| `MessagesController.java.patch` | Hide non-encrypted dialogs when E2E-only |
| `GroupCreateActivity.java.patch` | Block groups |
| `ChannelCreateActivity.java.patch` | Block channels |

### Config surface

- `UserConfig.getInstance(account).mg.e2eOnly`
- `UserConfig.getInstance(account).mg.useClassicSkin`
- Enabling E2E-only forces `preferSecretChats = true`

---

## Feature specs

### 1. E2E ↔ Normal mode

- Boolean (or enum) already exists.
- On → Normal: `sortDialogs` + `dialogsNeedReload`; allow groups/channels.
- On → E2E: hide non-secret; block create; confirmation once.
- Harden: search, contacts, profile, notification taps, deep links.

### 2. Classic skin

- Flag: `useClassicSkin` (done).
- Implement via Theme / resource overlay; keep modern fully working.
- MVP classic: action bar + list density + optional classic empty/welcome.
- Not required: pixel-perfect 2013 clone on day one.

### 3. BitChat (implement next among big items)

See [BITCHAT.md](BITCHAT.md).

**MVP only:**

1. Settings → “Nearby mesh / BitChat”
2. Toggle “Show BitChat entry points”
3. Drawer or FAB: “Open BitChat” → `Intent` to `com.bitchat.droid` or market/GitHub URI
4. Short privacy note (persistent mesh ID observable nearby)

Do not embed BLE stack until companion bridge ships.

### 4. Multi-protocol (Matrix, Fediverse, AI, location)

Documented in RESEARCH.md. **Do not start Matrix/Fediverse SDK work** until P0–P2 are solid and the user confirms they want to leave pure-patch architecture.

**AI assistant MVP (when scheduled):** separate activity, OpenAI-compatible base URL + key, no automatic Telegram content upload.

**Location search MVP:** Nominatim (or configurable) query UI → geo/share into chat; independent of Telegram places API.

### 5. Side-by-side with Telegram

Change package/`applicationId` in the build (Mercurygram Gradle) so E2EGram and official Telegram can both be installed. Document in README.

---

## Secrets & build

| Secret | Required for login |
|--------|---------------------|
| `APP_ID` | Yes |
| `APP_HASH` | Yes |

Source: https://my.telegram.org — **never commit**.  
Trigger: push to `main` or Actions → Build E2EGram APK → Run workflow.

---

## Agent rules

1. Read HANDOFF.md, RESEARCH.md, BITCHAT.md, README.md first.
2. Small patches only; verify `patch -p1` against current Mercurygram.
3. Never commit API credentials or AI keys.
4. Update Done / Not done and “Last handoff update” after meaningful work.
5. Multi-protocol work needs an explicit architecture decision — do not force Matrix into MTProto patches.
6. Classic skin = overlay; modern must keep working.

---

## Suggested work order

1. Confirm secrets + successful APK login smoke test  
2. E2E mode: confirmation + open-path hardening  
3. Distinct applicationId (side-by-side)  
4. BitChat companion MVP (settings + intent)  
5. Classic theme MVP (Theme/DialogCell)  
6. Independent location search (Nominatim)  
7. AI assistant activity (OpenAI-compatible)  
8. Only then: Matrix/Fediverse design doc + first bridge  

---

## Quick continue prompt

```
Continue E2EGram from HANDOFF.md and RESEARCH.md.

Repo: https://github.com/imanbarati/E2EGram
Base: Mercurygram (branch Mercurygram)

Priority order:
1) E2E mode hardening + confirmation
2) BitChat companion intent (BITCHAT.md)
3) Classic skin theme (flag already exists)
4) Side-by-side applicationId

Do not start Matrix/Fediverse SDKs until P0–P2 are done.
Keep changes as minimal CI patches. Never commit APP_ID/APP_HASH.
Update HANDOFF Done/Not done when finished.
```

---

*End of handoff. Update when you finish a chunk of work.*
