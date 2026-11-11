# BitChat integration plan for E2EGram

## What BitChat is

[BitChat](https://github.com/permissionlesstech/bitchat) / [bitchat-android](https://github.com/permissionlesstech/bitchat-android):

- Bluetooth LE mesh for offline multi-hop chat (~7 hops)
- Optional Nostr when internet exists
- No accounts / no phone numbers
- Private messages: Noise Protocol on mesh
- Public domain (Unlicense)

Nearby radios can observe a device-derived identifier — document that in UI.

## Why it fits

| Mode | Transport | Trust boundary |
|------|-----------|----------------|
| Normal Telegram | MTProto cloud | Telegram can read cloud chats |
| E2E only | Telegram secret chats | Device-to-device MTProto E2E |
| BitChat mesh | BLE (+ optional Nostr) | No Telegram involvement |

## Approaches

### A — Companion app bridge (MVP)

Intent to open BitChat Android app / share text. Fast, survives rebases.

### B — In-process library

Vendor bitchat-mesh or BLE stack. One APK; heavy maintenance.

### C — Plugin APK

Like Mercurygram Tor plugin: optional mesh plugin.

## MVP for next agent

1. Settings → BitChat / Nearby mesh section
2. Toggle enable entry points
3. Drawer action: Open BitChat (Approach A)
4. In-app explainer + privacy note
5. Do not claim BitChat messages appear inside Telegram threads until B/C exists

## References

- https://github.com/permissionlesstech/bitchat
- https://github.com/permissionlesstech/bitchat-android
- https://github.com/trancee/bitchat-mesh
