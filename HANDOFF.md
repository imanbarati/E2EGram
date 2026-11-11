# HANDOFF — E2EGram

**For any AI agent:** Read fully before changing code.

| Field | Value |
|-------|--------|
| **Repo** | https://github.com/imanbarati/E2EGram |
| **Base** | Mercurygram (`Mercurygram` branch) |
| **Package** | `org.e2egram.app` (side-by-side) |
| **Last update** | 2026-11-11 |

## Done

- [x] E2E-only mode: dialog filter, block group/channel create, force secret on new write
- [x] Confirmation dialog when enabling E2E-only
- [x] ProfileActivity: openChat → secret; openGroup blocked
- [x] ContactsActivity: force `createSecretChat` when E2E-only
- [x] LaunchActivity: user notification → existing secret chat or `startSecretChat`
- [x] Classic skin flag + BitChat companion settings/intent
- [x] CI builds arm64 debug APK + pre-release

## Not done

- [ ] Deep links / global search remaining edges
- [ ] Group notification ignore when E2E-only (user path done)
- [ ] Classic Theme visual overlay
- [ ] String resources
- [ ] Smoke-test with real APP_ID/APP_HASH
- [ ] P3–P4: location search, AI assistant, Matrix/Fediverse

## Patches

`MgAccountConfig`, `MercurygramSettingsActivity`, `DialogsActivity`, `ProfileActivity`, `ContactsActivity`, `LaunchActivity`, `MessagesController`, `GroupCreateActivity`, `ChannelCreateActivity`

## Next

1. Classic Theme MVP or deep-link edges
2. Verify CI APK + login
3. Optional: ignore `push_chat_id` when E2E-only

See RESEARCH.md / BITCHAT.md for longer-term direction.
