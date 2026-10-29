# HANDOFF — E2EGram

**For any AI agent (Claude, Grok, Cursor, etc.):** Read this file fully before changing code. Treat it as ground truth. Do not re-scaffold the project.

**Repo:** https://github.com/imanbarati/E2EGram  
**Owner:** imanbarati  
**Branch:** main  
**Last handoff update:** 2026-10-29

---

## Mission

Mercurygram fork with an experimental **E2E-only mode**:

- Hides normal cloud chats from the main list
- Forces new 1:1 chats into secret (end-to-end encrypted) chats
- Blocks creating groups and channels

Toggle lives under: **Settings → Mercurygram → E2E only mode (experimental)**

Every push to `main` (and manual **Actions → Build E2EGram APK → Run workflow**) builds a **debug arm64 APK** and publishes it as a pre-release.

---

## Repo contents

- `*.java.patch` — E2E-only changes applied on top of Mercurygram
- `.github/workflows/build.yml` — auto-build + release workflow
- `README.md` — human-facing overview

---

## What is DONE

- [x] Repo created and scaffolded
- [x] E2E-only patches added
- [x] Auto APK build workflow on push to main
- [x] README documenting secrets and behavior

---

## What is LEFT (ordered priority)

### P0 — Required for a usable APK login

1. **Add GitHub Actions secrets** (Settings → Secrets and variables → Actions):
   - `APP_ID` — Telegram API ID from https://my.telegram.org
   - `APP_HASH` — Telegram API hash from the same page
2. Without these secrets the APK still builds but **cannot log in to Telegram**.
3. Trigger a build (push any small change, or run the workflow manually) and download the pre-release APK.

### P1 — Verification

4. Install the debug arm64 APK on a device/emulator.
5. Confirm the E2E-only toggle appears and works:
   - Cloud chats hidden from main list
   - New 1:1 chats become secret chats
   - Groups/channels creation blocked
6. Document any crashes, UI glitches, or Mercurygram upstream conflicts in this HANDOFF.

### P2 — Hardening (optional)

7. Keep patches minimal and re-apply cleanly if Mercurygram upstream moves.
8. Consider a release-signed APK path (not required for experimental use).
9. Update this HANDOFF after meaningful work (Done / Left / known issues).

---

## Agent rules

1. Do **not** commit real `APP_ID` / `APP_HASH` values into the repo. Secrets stay in GitHub Actions secrets only.
2. Prefer small, reversible patch changes. Do not rewrite large parts of Mercurygram.
3. After any successful build or behavior change, update the **Done / Left** sections above.
4. If the build workflow fails, open the Actions log first; most failures are missing secrets or Java/Android toolchain issues in the runner.

---

## Quick continue prompt (paste into a new chat)

```
Continue E2EGram from HANDOFF.md.

Repo: https://github.com/imanbarati/E2EGram (owner imanbarati, branch main)

1. Read HANDOFF.md and README.md first.
2. Highest priority: ensure APP_ID and APP_HASH secrets exist, then trigger/build the debug APK and verify E2E-only mode.
3. Do not commit Telegram API credentials.
4. After work, update HANDOFF.md Done/Left sections.
```

---

*End of handoff.*
