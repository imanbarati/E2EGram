# BitChat integration plan for E2EGram

Last updated: 2026-11-11

## What BitChat is

- [bitchat-android](https://github.com/permissionlesstech/bitchat-android) (~8k stars)
- Bluetooth LE mesh, multi-hop (~7), offline-first
- Optional Nostr for internet reach
- No accounts, no phone numbers
- Private messages: Noise Protocol on mesh
- Android package typically `com.bitchat.droid`

Privacy note for UI: nearby radios can observe a device-derived persistent identifier. Document that honestly.

## Fit with E2EGram

| Mode | Transport | Trust |
|------|-----------|--------|
| Normal Telegram | MTProto cloud | Telegram can read cloud chats |
| E2E only | Telegram secret chats | Device-to-device MTProto |
| BitChat | BLE mesh (+ optional Nostr) | No Telegram servers |

## MVP — implement this (companion bridge)

Do **not** vendor the BLE stack in the first iteration.

1. **Config**  
   - Optional: `mg.bitChatEnabled` boolean in `MgAccountConfig` (default true or false)

2. **Settings** (MercurygramSettingsActivity)  
   - Section: “Nearby mesh / BitChat”  
   - Toggle: enable entry points  
   - Button: “Open BitChat”  
   - Shadow: one-sentence privacy note + “Opens the BitChat app if installed”

3. **Intent**  
   - Prefer launch by package `com.bitchat.droid`  
   - Fallback: https://github.com/permissionlesstech/bitchat-android/releases or Play listing  
   - Optional: `ACTION_SEND` text share into BitChat when user picks “Share to mesh”

4. **Drawer / chat list**  
   - Menu item or FAB action “BitChat” when enabled

5. **Strings**  
   - Prefer resources over hard-coded English when touching this area

## Later (not MVP)

- In-process mesh via [bitchat-mesh](https://github.com/trancee/bitchat-mesh) or extracted stack  
- Plugin APK pattern (like Mercurygram Tor)  
- Showing BitChat threads inside the Telegram dialog list  

## References

- https://github.com/permissionlesstech/bitchat  
- https://github.com/permissionlesstech/bitchat-android  
- https://github.com/trancee/bitchat-mesh  
