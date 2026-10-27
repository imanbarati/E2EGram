# E2EGram

Mercurygram + experimental **E2E-only mode**.

## What it does

Adds a toggle under **Settings → Mercurygram → E2E only mode (experimental)**:

- Hides normal cloud chats from the main list
- Forces new 1:1 chats into secret (end-to-end encrypted) chats
- Blocks creating groups and channels

## Automatic APK builds

Every push to `main` (and manual run from the Actions tab) builds a **debug arm64 APK** and:

1. Uploads it as a workflow artifact
2. Publishes it as a pre-release under Releases

### Required secrets (recommended)

Go to **Settings → Secrets and variables → Actions** and add:

| Secret     | Description                                      |
|------------|--------------------------------------------------|
| `APP_ID`   | Your Telegram API ID (from https://my.telegram.org) |
| `APP_HASH` | Your Telegram API hash                           |

Without them the build still runs but uses placeholders — the app will not connect to Telegram until you set real values.

## Files

- `MgAccountConfig.java` – e2eOnly flag
- `MercurygramSettingsActivity.java` – settings toggle
- `DialogsActivity.java` – force secret chat on new chats
- `MessagesController.java` – hide non-secret dialogs
- `GroupCreateActivity.java` / `ChannelCreateActivity.java` – block group/channel creation
- `.github/workflows/build.yml` – auto-build workflow

## Manual local build

```bash
git clone --depth 1 -b Mercurygram https://github.com/Mercurygram/Mercurygram.git
cd Mercurygram
# copy the 6 .java files from this repo over the matching paths
./gradlew assembleAfatFdArm64Debug
```
