# E2EGram

Mercurygram + **E2E-only mode**, classic/modern skin flag, BitChat entry points — tracking Telegram upstream via Mercurygram.

**Current upstream:** Mercurygram **13.2.1.x** (Telegram **13.2.1**).

**Package id:** `org.e2egram.app` — installs **side-by-side** with official Telegram and Mercurygram (`it.belloworld.mercurygram`).

## What it does

Under **Settings → Mercurygram**:

| Control | Behaviour |
|---------|-----------|
| **Chat mode: E2E only** | Secret chats only; cloud chats hidden; groups/channels blocked |
| **Chat mode: Normal Telegram** | Full Mercurygram behaviour |
| **Skin: Classic / Modern** | Classic flag for early Telegram look (theme overlay still experimental) |
| **BitChat / nearby mesh** | Opens the [BitChat](https://github.com/permissionlesstech/bitchat-android) companion app (BLE mesh, no Telegram servers) |

## Automatic APK builds

Every push to `main` (and **Actions → Build E2EGram APK → Run workflow**) builds a **debug arm64 APK** and publishes it as a pre-release.

### Add Telegram API keys (required to log in)

1. Open https://my.telegram.org → log in with your phone number  
2. Go to **API development tools** → create an app if needed  
3. Copy **api_id** and **api_hash**  
4. In this GitHub repo: **Settings → Secrets and variables → Actions → New repository secret**  
   - Name: `APP_ID` → value = your api_id  
   - Name: `APP_HASH` → value = your api_hash  
5. Run the workflow again

Without these secrets the APK still builds but **cannot log in to Telegram**.

**Never commit APP_ID / APP_HASH into the repository.**

## Repo contents

- `*.java.patch` — applied on top of latest Mercurygram at build time  
- `gradle` package override → `org.e2egram.app`  
- [HANDOFF.md](HANDOFF.md) — instructions for AI agents  
- [RESEARCH.md](RESEARCH.md) — forks + multi-protocol roadmap  
- [BITCHAT.md](BITCHAT.md) — mesh integration plan  

## Manual local build

```bash
git clone --depth 1 -b Mercurygram https://github.com/Mercurygram/Mercurygram.git
cd Mercurygram
# apply all *.java.patch files with: patch -p1 < path/to/patch
sed -i 's/^APP_PACKAGE=.*/APP_PACKAGE=org.e2egram.app/' gradle.properties
# put APP_ID / APP_HASH into API_KEYS file
./gradlew assembleAfatFdArm64Debug
```

## About

Privacy-focused Mercurygram scaffold: hard E2E-only mode, side-by-side install, BitChat bridge, classic skin flag. See HANDOFF for Matrix/Fediverse/AI longer-term direction.
