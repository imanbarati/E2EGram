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

Build a **switchable** Mercurygram-based client with three independent controls:

1. **Chat mode**
   - **E2E-only mode** — only secret (end-to-end encrypted) chats; hide cloud chats; block groups/channels
   - **Normal Telegram mode** — full Mercurygram / Telegram behaviour (cloud chats, groups, channels)

2. **Skin / UI style**
   - **Classic (old) skin** — early Telegram look (paper-plane welcome, simple list, pre-Material / early iOS-style aesthetic shown in the 2013–2014 era screenshots)
   - **Modern (new) skin** — current Mercurygram / upstream Telegram Material You style

Both switches must be runtime toggles (not separate APKs). User can flip them independently under **Settings → Mercurygram** (or a dedicated Appearance / Privacy section).

---

## Current state (what already exists)

### Done

- [x] Repo created: https://github.com/imanbarati/E2EGram
- [x] E2E-only mode scaffold as `.java.patch` files applied on top of Mercurygram at build time
- [x] GitHub Actions workflow (`.github/workflows/build.yml`) that:
  1. Checks out Mercurygram
  2. Applies the patches
  3. Builds a **debug arm64 APK**
  4. Uploads artifact + creates a pre-release
- [x] Toggle for E2E-only under Settings → Mercurygram (flag `e2eOnly` in `MgAccountConfig`)
- [x] Behaviour when `e2eOnly == true`:
  - Non-secret dialogs filtered out in `MessagesController.sortDialogs`
  - New chats from write button forced to secret chat (`DialogsActivity`)
  - Group / channel creation blocked (`GroupCreateActivity`, `ChannelCreateActivity`)

### Not done yet

- [ ] **Mode switch UI** that clearly lets the user pick “E2E only” vs “Normal Telegram” (current toggle is experimental and one-way-ish)
- [ ] **Classic / modern skin switch**
- [ ] Force-open path: opening an existing contact still routes into a secret chat when E2E mode is on
- [ ] Proper string resources (currently some hard-coded English)
- [ ] Verification that the GitHub Actions build produces a installable APK with real `APP_ID` / `APP_HASH`
- [ ] Optional distinct `applicationId` so E2EGram can install side-by-side with official Telegram / Mercurygram

---

## Architecture notes (important for agents)

### How the build works

This repo does **not** contain a full Telegram source tree.  
At CI time:

1. Checkout `Mercurygram/Mercurygram` @ branch `Mercurygram` into `src/`
2. Apply the `*.java.patch` files with `patch -p1`
3. Build with Gradle (`assembleAfatFdArm64Debug` preferred)

So every change must remain a **minimal, re-applicable patch** against current Mercurygram. Prefer editing the existing patches or adding new small `.patch` files. Do not vendor the whole TMessagesProj tree unless the user explicitly asks.

### Key files already patched

| Patch file | Target in Mercurygram | Purpose |
|------------|------------------------|---------|
| `MgAccountConfig.java.patch` | `it/belloworld/mercurygram/MgAccountConfig.java` | Adds `boolean e2eOnly` + save/load/reset |
| `MercurygramSettingsActivity.java.patch` | `.../MercurygramSettingsActivity.java` | Adds the E2E-only settings toggle |
| `DialogsActivity.java.patch` | `org/telegram/ui/DialogsActivity.java` | Forces secret chat on new write |
| `MessagesController.java.patch` | `org/telegram/messenger/MessagesController.java` | Hides non-encrypted dialogs when `e2eOnly` |
| `GroupCreateActivity.java.patch` | `org/telegram/ui/GroupCreateActivity.java` | Blocks group creation |
| `ChannelCreateActivity.java.patch` | `org/telegram/ui/ChannelCreateActivity.java` | Blocks channel creation |

### Config surface

- Per-account flag: `UserConfig.getInstance(account).mg.e2eOnly`
- Also forces `preferSecretChats = true` when turning E2E mode on
- Settings UI entry: `ID_E2E_ONLY` in `MercurygramSettingsActivity`

---

## Feature specs to implement next

### 1. Explicit mode switch (E2E ↔ Normal)

**Goal:** User can freely switch between:

- **E2E only** — current behaviour (secret chats only)
- **Normal** — full cloud chats, groups, channels (Mercurygram default)

**Suggested implementation:**

- Keep `e2eOnly` boolean (or promote to an enum `ChatMode { E2E_ONLY, NORMAL }` if cleaner)
- When switching **to Normal**: re-show all dialogs (call `sortDialogs` / post `dialogsNeedReload`), allow group/channel creation again
- When switching **to E2E only**: hide non-secret dialogs, force secret-chat creation paths, block groups/channels
- Show a confirmation dialog the first time the user enables E2E-only (explain that cloud history stays on Telegram servers and is only hidden locally)
- Persist choice in `MgAccountConfig` (already done for the boolean)

**Open paths still to harden when E2E-only is on:**

- Opening a user from search / contacts / profile → force secret chat or refuse
- Notifications that open a cloud chat → redirect or show “switch to secret chat”
- Deep links / share targets

### 2. Classic vs modern skin switch

**Goal:** Runtime toggle between two visual styles.

**Classic (old) skin reference** (see attached screenshot in conversation):

- Early Telegram welcome screen: paper-plane logo on blue sky + sunflower field, “Welcome to Telegram — the world's fastest messaging app. It is free and secure.”, green “Start Messaging” button
- Simple chat list (iOS-style navigation bar “Messages”, search, basic rows with avatar + name + last message + time)
- Minimal Material; closer to 2013–2014 Telegram / early iOS Messages aesthetic

**Modern skin:** current Mercurygram / upstream Telegram (Material You, folders, stories, etc.)

**Implementation guidance:**

- Add a setting, e.g. `mg.useClassicSkin` (boolean) or `mg.skinStyle` (enum)
- Prefer theme / resource overlay approach rather than forking every Activity:
  - Theme keys in `Theme.java` / `ThemeDescription`
  - Optional alternate layouts or drawable sets under a classic resource qualifier or runtime swap
- Classic mode does **not** need pixel-perfect 2013 recreation on day one. Minimum viable:
  - Simpler chat list row layout
  - Classic-style action bar colours / fonts where easy
  - Optional classic welcome / empty state
- Do not remove modern features (folders, stories, etc.) when classic skin is on unless the user later asks for a strict classic-only experience; just change the look.

**Where to look in Mercurygram / Telegram Android:**

- `Theme.java`, `ThemeColors`, chat list cell classes (`DialogCell`, etc.)
- `DialogsActivity` / `DialogsAdapter` for list appearance
- Launch / intro activities for the welcome screen

### 3. Independent switches

Skin choice and chat mode must be **independent**:

| | E2E only | Normal |
|--|----------|--------|
| **Classic skin** | Classic UI + secret chats only | Classic UI + full Telegram |
| **Modern skin** | Modern UI + secret chats only | Modern UI + full Telegram (default) |

---

## Secrets & build

| Secret | Required? | Source |
|--------|-----------|--------|
| `APP_ID` | Yes for login | https://my.telegram.org |
| `APP_HASH` | Yes for login | https://my.telegram.org |

Without them the APK builds but cannot connect to Telegram.

**Never commit real APP_ID / APP_HASH.** Use GitHub Actions secrets only.

Trigger build: push to `main` or **Actions → Build E2EGram APK → Run workflow**.

---

## Agent rules

1. Read this HANDOFF and `README.md` before any code change.
2. Prefer small, reversible patches. Re-test that `patch -p1` still applies cleanly against current Mercurygram.
3. Do not commit Telegram API credentials.
4. Do not rewrite large parts of Mercurygram; stay in the patch layer.
5. After meaningful work, update the **Done / Not done** sections of this file and bump the “Last handoff update” date.
6. If the build fails, open the GitHub Actions log first (missing secrets, NDK version, patch reject are the usual causes).
7. When implementing the classic skin, keep modern mode fully working; classic is an overlay/theme choice, not a permanent fork of the UI code.

---

## Suggested work order for the next agent

1. Confirm `APP_ID` / `APP_HASH` secrets exist; run a successful APK build; smoke-test the existing E2E-only toggle.
2. Improve the mode switch UX (clear labels: “E2E only” vs “Normal Telegram”, confirmation on enable, instant list refresh).
3. Harden “open user” paths so E2E-only truly forces secret chats.
4. Add `useClassicSkin` (or equivalent) flag + settings row.
5. Implement a minimal classic theme (chat list + action bar) that can be toggled at runtime.
6. Optional: classic-style welcome / empty state.
7. Update this HANDOFF after each milestone.

---

## Quick continue prompt (paste into a new chat with Gemini / Claude / Grok)

```
Continue E2EGram from HANDOFF.md.

Repo: https://github.com/imanbarati/E2EGram (owner imanbarati, branch main)
Base: Mercurygram (branch Mercurygram)

Goals:
1. Runtime switch between E2E-only mode and normal Telegram mode (already partially done via e2eOnly).
2. Runtime switch between classic (old 2013-style) skin and modern Mercurygram skin.
3. Keep changes as minimal patches applied at CI time; do not vendor the full Telegram tree.
4. Never commit APP_ID / APP_HASH.

Read HANDOFF.md and README.md first, then implement the highest-priority missing item and update HANDOFF.md Done/Left sections.
```

---

## Reference image (classic skin target)

Early Telegram (circa 2013–2014): paper-plane logo over blue sky and sunflower field, “Welcome to Telegram — the world's fastest messaging app. It is free and secure.”, green Start Messaging button, and a simple Messages list (Paul Durov, phone number, Kim Dotcom, Mark Z, etc.). Use that aesthetic as the classic-skin target, not a pixel-perfect clone on the first iteration.

---

*End of handoff. Update this file when you finish a chunk of work.*
