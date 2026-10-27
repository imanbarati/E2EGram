# E2EGram

Mercurygram + experimental **E2E-only mode**.

## What it does

Adds a toggle under **Settings → Mercurygram → E2E only mode (experimental)**:

- Hides normal cloud chats from the main list
- Forces new 1:1 chats into secret (end-to-end encrypted) chats
- Blocks creating groups and channels

## Automatic APK builds

Every push to `main` (and **Actions → Build E2EGram APK → Run workflow**) builds a **debug arm64 APK** and publishes it as a pre-release.

### Secrets (recommended)

**Settings → Secrets and variables → Actions**:

| Secret | Description |
|--------|-------------|
| `APP_ID` | Telegram API ID from https://my.telegram.org |
| `APP_HASH` | Telegram API hash |

Without them the APK builds but cannot log in to Telegram.

## Repo contents

- `*.java.patch` — the E2E-only changes applied on top of Mercurygram
- `.github/workflows/build.yml` — auto-build + release workflow
