# Research: E2EGram roadmap, forks & multi-protocol direction

Last updated: 2026-11-11

## Positioning (updated)

**E2EGram vision (user direction 2026-11-11):**  
A privacy-first client that can **run side-by-side with Telegram** while offering:

- Hard E2E-only mode on Telegram transport (secret chats)
- Classic ↔ modern skin
- Offline Bluetooth mesh (BitChat)
- Long-term: Matrix, Fediverse (Pleroma / Soapbox / ActivityPub), independent location search, and OpenAI-compatible AI assistants (Claude, Muse, etc.)

**Reality check:** The current codebase is a **patch layer on Mercurygram** (Telegram Android / MTProto). Matrix, ActivityPub, and full AI-assistant shells are **different protocol stacks**. They cannot be “small patches.” They require either:

1. **Unified multi-protocol shell** (new architecture: Telegram is one backend among many), or  
2. **Companion modules / intents** (open Matrix client, Fediverse client, BitChat, AI chat as separate activities or apps), or  
3. **Hybrid** — keep Mercurygram for Telegram + embed lightweight SDKs for the rest over time.

Claiming “fully replace Telegram” while also “run seamlessly alongside Telegram” means: **multi-account, multi-protocol unified UI**, not deleting MTProto.

---

## Top Android Telegram forks (2026 landscape)

| Client | Focus | Notable features | Fit for E2EGram |
|--------|--------|------------------|-----------------|
| **Mercurygram** (base) | FOSS / de-Google | UnifiedPush, no GMS, OSM, Tor plugin | Keep as Telegram backend |
| **Forkgram** | Small UX | Delete-for-all default, clean network profile | Steal polish |
| **Nekogram / Nagram / NagramX** | Power-user | Proxies, multi-account, dense UI | Ideas only |
| **AyuGram** | Ghost / anti-delete | Hide online/typing/read; keep deleted msgs | High demand; ToS-sensitive |
| **OpenGram** | Privacy filters | Disable remote deletion, content filters | Aligns with E2E ethos |
| **exteraGram / exteraless** | Modern + plugins | Plugin permission model, Material player | Plugin pattern inspiration |
| **Telegram X** | Official alt | TDLib, snappy | Different stack |
| **zkgram** (desktop) | Extra E2E layer | Independent crypto over Telegram transport | Long-term crypto ideas |

**Takeaway:** Stay FOSS + E2E-focused. Do not become a kitchen-sink Nekogram clone.

---

## Other protocols & services (new scope)

### Matrix
- Open standard for decentralized E2EE messaging (Olm/Megolm).
- Mature Android clients: Element, FluffyChat, SchildiChat.
- **Integration paths for E2EGram:**
  - **MVP:** Intent / deep-link to Element (or user-chosen Matrix client).
  - **Medium:** Embed a Matrix SDK (e.g. matrix-android-sdk2) as a separate account type in a unified inbox — large effort.
  - **Hard:** Full native Matrix UI inside E2EGram.

### Fediverse / ActivityPub (Pleroma, Soapbox, Mastodon, etc.)
- Federated social + DMs; not a 1:1 messenger replacement for Telegram groups/channels.
- **Soapbox** and **Pleroma** are common self-hosted backends; clients talk ActivityPub / Mastodon-compatible APIs.
- **Integration paths:**
  - **MVP:** Open Fediverse client or in-app WebView to a chosen instance.
  - **Medium:** Add a “Fediverse account” type using a Mastodon/Pleroma API library; timeline + DMs in a separate tab.
  - Full seamless mixed timeline with Telegram is a product decision, not a weekend patch.

### BitChat (Bluetooth mesh + Nostr)
- See [BITCHAT.md](BITCHAT.md).
- No accounts, BLE multi-hop (~7), Noise E2E on mesh, optional Nostr.
- **MVP:** Companion app bridge (intent to BitChat Android).
- **Later:** Optional in-process or plugin module.

### Independent location search (not Telegram)
- Telegram location = Telegram’s places / Google / FOSS maps depending on build.
- **Independent** means: web search / OSM / Nominatim / other APIs inside E2EGram, results not tied to Telegram’s server-side place database.
- **MVP:** Settings or compose-location flow → query Nominatim (OSM) or a configurable search API → show results in-app → optional share as text/geo URI into a chat.
- Fits Mercurygram’s existing OSM preference.

### OpenAI-compatible clients (Claude, Muse, local LLMs, etc.)
- These are **chat-completion API clients**, not messengers.
- **MVP:** Dedicated “AI assistant” section: base URL + API key (OpenAI-compatible), model picker, simple multi-turn chat UI. Works with OpenAI, Claude (via compatible proxy), local (Ollama/vLLM), Muse, etc.
- Do **not** send Telegram message content to AI APIs by default; explicit user action only.
- Keep keys in encrypted storage / Android Keystore; never log them.

---

## Recommended roadmap

### P0 — Core identity (current architecture)

1. Clear **E2E only ↔ Normal Telegram** switch (labels improved; confirmation + open-path hardening still needed)
2. **Classic ↔ modern skin** — flag exists (`useClassicSkin`); theme application TBD
3. Force secret-chat open paths when E2E-only
4. APP_ID / APP_HASH via CI secrets (documented)
5. Distinct `applicationId` so E2EGram installs **side-by-side** with Telegram / Mercurygram

### P1 — Privacy differentiators

6. Optional Ghost mode (typing / online / read) — off by default  
7. Keep deleted messages locally (OpenGram-style)  
8. Contacts sync off by default  
9. Stronger secret-chat defaults  

### P2 — Offline / mesh

10. **BitChat** companion bridge (settings + drawer “Open BitChat”) — see BITCHAT.md  

### P3 — Adjacent features (still Telegram-centric)

11. Independent location search (Nominatim / web)  
12. Unlimited local pins, channel content filters  
13. In-app OpenAI-compatible AI assistant (separate from chats)  

### P4 — Multi-protocol (architecture change)

14. Matrix: intent bridge → then optional SDK account type  
15. Fediverse (Pleroma / Soapbox / Mastodon API): intent or WebView → then account type  
16. Unified inbox design (Telegram + Matrix + Fediverse + BitChat + AI) — product + eng redesign  

### Explicitly deprioritize

- Full Nekogram feature dump  
- Closed-source / reverse-engineered blobs  
- Claiming cloud Telegram chats are E2E without an extra crypto layer  
- Pretending Matrix/Fediverse can be done as small `.java.patch` files on MTProto  

---

## Architecture recommendation for “side by side + replace”

| Goal | Approach |
|------|----------|
| Install next to official Telegram | Change `applicationId` / package name in build |
| E2E-only Telegram | Current `e2eOnly` flag + hardening |
| Classic look | Theme overlay on Mercurygram |
| BitChat | Companion intent first |
| Matrix / Fediverse | Separate backends behind a multi-protocol account model |
| AI assistants | Separate activity; OpenAI-compatible HTTP client |
| “Replace Telegram” | Long-term unified client; short-term best-in-class Telegram FOSS + bridges |

Do **not** attempt to reimplement MTProto, Matrix, and ActivityPub in one giant patch set. Prefer:

1. Finish P0–P2 on the current Mercurygram patch layer.  
2. Introduce a thin “Accounts / Networks” abstraction when adding the first non-Telegram protocol.  
3. Keep CI patch-apply workflow until the project intentionally becomes a multi-repo or full tree.

---

## References

- Mercurygram: https://github.com/Mercurygram/Mercurygram  
- BitChat Android: https://github.com/permissionlesstech/bitchat-android  
- Matrix Android SDK: https://github.com/matrix-org/matrix-android-sdk2  
- Pleroma / Soapbox: ActivityPub; Mastodon-compatible API clients  
- Nominatim (OSM search): https://nominatim.org/  
