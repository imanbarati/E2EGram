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
- [x] `useClassicSkin` flag in `MgAccountConfig` + settings row (full Theme overlay still experimental)
- [x] Distinct package id **`org.e2egram.app`** (CI sed on `APP_PACKAGE`) — side-by-side with Telegram/Mercurygram
- [x] BitChat MVP companion: `bitChatEnabled` + Settings toggle + **Open BitChat** intent (`com.bitchat.droid` / GitHub releases fallback)
- [x] RESEARCH.md, BITCHAT.md, this HANDOFF
- [x] APP_ID / APP_HASH documented as GitHub Actions secrets

### Not done (P0–P2)

- [ ] Confirmation dialog on first enable of E2E-only
- [ ] Harden open paths (search/profile/notifications) to force secret chat when E2E-only
- [ ] Classic **theme** application (Theme / DialogCell / intro) — flag + settings done; visual overlay TBD
- [ ] String resources instead of hard-coded English
- [ ] Drawer/FAB BitChat entry (settings button exists)
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
3. Sets `APP_PACKAGE=org.e2egram.app` via sed
4. Builds `assembleAfatFdArm64Debug`

**Rule:** Prefer small reversible patches. Do not vendor the full Telegram tree unless the user explicitly decides to leave the patch model.

### Key patches

| Patch | Purpose |
|-------|---------|
| `MgAccountConfig.java.patch` | `e2eOnly`, `useClassicSkin`, `bitChatEnabled` |
| `MercurygramSettingsActivity.java.patch` | Settings rows + handlers + BitChat open intent |
| `DialogsActivity.java.patch` | Force secret on new write |
| `MessagesController.java.patch` | Hide non-encrypted dialogs when E2E-only |
| `GroupCreateActivity.java.patch` | Block groups |
| `ChannelCreateActivity.java.patch` | Block channels |

### Config surface

- `UserConfig.getInstance(account).mg.e2eOnly`
- `UserConfig.getInstance(account).mg.useClassicSkin`
- `UserConfig.getInstance(account).mg.bitChatEnabled`
- Enabling E2E-only forces `preferSecretChats = true`

---

## Feature specs

### 1. E2E ↔ Normal mode

- Boolean already exists.
- On → Normal: `sortDialogs` + `dialogsNeedReload`; allow groups/channels.
- On → E2E: hide non-secret; block create; confirmation once (still TODO).
- Harden: search, contacts, profile, notification taps, deep links.

### 2. Classic skin

- Flag: `useClassicSkin` (done).
- Implement via Theme / resource overlay; keep modern fully working.
- MVP classic: action bar + list density + optional classic empty/welcome.

### 3. BitChat

See [BITCHAT.md](BITCHAT.md). **MVP shipped:** settings toggle + Open BitChat intent.

### 4. Multi-protocol (Matrix, Fediverse, AI, location)

Documented in RESEARCH.md. Do not start SDK work until P0–P2 are solid.

### 5. Side-by-side with Telegram

**Done:** `APP_PACKAGE=org.e2egram.app` in CI.

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
4. Update Done / Not done after meaningful work.
5. Multi-protocol work needs an explicit architecture decision.
6. Classic skin = overlay; modern must keep working.

---

## Suggested work order

1. Confirm secrets + successful APK login smoke test  
2. E2E mode: confirmation + open-path hardening  
3. Classic theme MVP (Theme/DialogCell)  
4. Independent location search (Nominatim)  
5. AI assistant activity (OpenAI-compatible)  
6. Only then: Matrix/Fediverse design doc + first bridge  

---

*End of handoff.*
