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
2. Can install **side-by-side** with official Telegram (`org.e2egram.app`)
3. Bridges to **BitChat** (offline mesh), and later Matrix / Fediverse / AI assistants
4. Does **not** pretend multi-protocol is a few patches on MTProto

See [RESEARCH.md](RESEARCH.md) and [BITCHAT.md](BITCHAT.md).

---

## Current state

### Done

- [x] Repo + CI patches → arm64 debug APK + pre-release
- [x] `e2eOnly`, `useClassicSkin`, `bitChatEnabled` in `MgAccountConfig`
- [x] Settings: E2E / Classic / BitChat rows
- [x] **Confirmation dialog when enabling E2E-only**
- [x] Dialog filter + force secret on new write (DialogsActivity)
- [x] Block group/channel create
- [x] **ProfileActivity**: `openChat` → secret chat when E2E-only; `openGroup` blocked
- [x] Package id `org.e2egram.app` (side-by-side)
- [x] BitChat companion intent from settings

### Not done

- [ ] Harden remaining paths (notifications, deep links, global search results)
- [ ] Classic **theme** visual overlay (flag only)
- [ ] String resources (hard-coded English in patches)
- [ ] Drawer/FAB BitChat entry
- [ ] Smoke-test login with real APP_ID/APP_HASH
- [ ] Independent location search / AI assistant / Matrix / Fediverse (P3–P4)

---

## Key patches

| Patch | Purpose |
|-------|---------|
| `MgAccountConfig.java.patch` | flags |
| `MercurygramSettingsActivity.java.patch` | UI + E2E confirm + BitChat open |
| `DialogsActivity.java.patch` | new write → secret |
| `ProfileActivity.java.patch` | profile open → secret / block groups |
| `MessagesController.java.patch` | hide non-secret dialogs |
| `GroupCreateActivity` / `ChannelCreateActivity` | block create |

---

## Suggested next work

1. Notification / LaunchActivity deep-link hardening when e2eOnly
2. Classic Theme overlay MVP
3. Smoke-test APK with secrets
4. Nominatim location search or AI assistant activity

---

*Update this file when you finish a chunk of work.*
