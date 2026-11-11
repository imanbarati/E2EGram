# E2EGram

Mercurygram + experimental **E2E-only mode**, tracking latest Telegram upstream via Mercurygram.

**Current upstream:** Mercurygram **13.2.1.1** (Telegram **13.2.1**) — patches rebased 2026-11-11.

## What it does

Toggle under **Settings → Mercurygram → E2E only mode (experimental)**:

- Hides normal cloud chats from the main list
- Forces new 1:1 chats into secret (end-to-end encrypted) chats
- Blocks creating groups and channels

Planned (see [HANDOFF.md](HANDOFF.md)):

- Clear switch between **E2E only** and **Normal Telegram**
- Switch between **classic (old)** and **modern** skins

## Automatic APK builds

Every push to `main` (and **Actions → Build E2EGram APK → Run workflow**) builds a **debug arm64 APK** and publishes it as a pre-release.

### Add Telegram API keys (required to log in)

1. Open https://my.telegram.org → log in with your phone number  
2. Go to **API development tools** → create an app if needed  
3. Copy **api_id** and **api_hash**  
4. In this GitHub repo: **Settings → Secrets and variables → Actions → New repository secret**  
   - Name: `APP_ID` → value = your api_id  
   - Name: `APP_HASH` → value = your api_hash  
5. Run the workflow again (Actions → Build E2EGram APK → Run workflow)

Without these secrets the APK still builds but **cannot log in to Telegram**.

**Never commit APP_ID / APP_HASH into the repository.**

## Repo contents

- `*.java.patch` — E2E-only changes applied on top of latest Mercurygram at build time  
- `.github/workflows/build.yml` — clones Mercurygram tip, applies patches, builds APK  
- [HANDOFF.md](HANDOFF.md) — full instructions for AI agents continuing development  

## Manual local build

```bash
git clone --depth 1 -b Mercurygram https://github.com/Mercurygram/Mercurygram.git
cd Mercurygram
# apply all *.java.patch files with: patch -p1 < path/to/patch
# put APP_ID / APP_HASH into API_KEYS file
./gradlew assembleAfatFdArm64Debug
```
