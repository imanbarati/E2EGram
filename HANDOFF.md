# HANDOFF — E2EGram

| Field | Value |
|-------|--------|
| Repo | https://github.com/imanbarati/E2EGram |
| Base | Mercurygram branch `Mercurygram` |
| Package | `org.e2egram.app` |
| Updated | 2026-11-14 |

## Done

- E2E-only mode + confirmation dialog
- Dialog filter, group/channel create block, secret on new write
- Profile / Contacts / notification (user) → secret chat paths
- Classic skin + BitChat flags in settings
- Settings: Open BitChat, Open Grokipedia, Open E2EGram blog
- Docs: BITCHAT.md (fork plan), BLOG.md (Astro + Cloudflare + EmDash)
- Clean regenerated patches (2026-11-14) verified with `patch --dry-run`

## Not done

- Classic Theme visual overlay
- Soft-fork repo `E2EGram-BitChat` + CI
- Deploy real Astro blog URL (replace pages.dev placeholder)
- Group notification ignore in E2E-only
- Matrix / Fediverse / AI assistant (see RESEARCH.md)

## Next agents

1. Confirm Actions build is green
2. Fork bitchat-android → E2EGram-BitChat
3. Scaffold e2egram-blog (Astro + CF) and set blog URL
4. Classic Theme hooks
