# HANDOFF addendum — 2026-11-11

## User priorities

1. Classic ↔ modern skin toggle (flag `useClassicSkin` scaffolded; theme TBD)
2. Cleaner E2E ↔ Normal mode switch UI (labels improved in settings patch)
3. Feature research → RESEARCH.md
4. BitChat → BITCHAT.md (MVP = companion bridge)

## Mode switch UX

Settings row shows **Chat mode: E2E only** or **Chat mode: Normal Telegram** depending on state. Shadow text explains both states. Confirmation dialog still recommended on first enable.

## Classic skin

`useClassicSkin` persisted. Toast acknowledges toggle until Theme/DialogCell work lands.

## Upstream

Mercurygram 13.2.1.1 / Telegram 13.2.1.
