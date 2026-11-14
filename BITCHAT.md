# BitChat + E2EGram

Last updated: 2026-11-14

## Upstream

- iOS/macOS: https://github.com/permissionlesstech/bitchat (Unlicense)
- Android: https://github.com/permissionlesstech/bitchat-android (GPL-3.0)
- Package ids: `com.bitchat.droid`, `com.bitchat.android`

## Strategy

### Phase A — Companion (in E2EGram now)

Settings → **Open BitChat** launches installed app or GitHub Releases.

### Phase B — Soft fork under your org (recommended next)

1. Fork `permissionlesstech/bitchat-android` → e.g. `imanbarati/E2EGram-BitChat`
2. Keep upstream sync; optional branding / applicationId `org.e2egram.bitchat`
3. CI builds release APK next to E2EGram
4. E2EGram intent prefers `org.e2egram.bitchat` then falls back to upstream package ids

### Phase C — Deeper integration (later)

- bitchat-mesh library / plugin APK pattern (like Mercurygram Tor)
- Do **not** dump the full BLE stack into Mercurygram patches

## Privacy note (show in UI)

Nearby radios can observe a device-derived mesh identifier.

## References

- https://github.com/permissionlesstech/bitchat-android
- https://github.com/trancee/bitchat-mesh
