# E2EGram

Mercurygram + hard **E2E-only** mode, BitChat companion entry, Grokipedia/blog links.

**Package:** `org.e2egram.app` (side-by-side with Telegram / Mercurygram)

## Settings → Mercurygram

| Control | Action |
|---------|--------|
| Chat mode E2E only | Secret chats only; confirmation on enable |
| Classic / Modern skin | Flag (theme overlay experimental) |
| BitChat | Open companion mesh app |
| Grokipedia | Open https://grokipedia.com |
| E2EGram blog | Open Astro/Cloudflare blog (see [BLOG.md](BLOG.md)) |

## Secrets

Set `APP_ID` and `APP_HASH` in GitHub Actions secrets (from https://my.telegram.org).

## Docs

- [HANDOFF.md](HANDOFF.md) — agent instructions
- [BITCHAT.md](BITCHAT.md) — mesh + fork plan
- [BLOG.md](BLOG.md) — Astro + Cloudflare + EmDash CMS plan
- [RESEARCH.md](RESEARCH.md) — multi-protocol roadmap
