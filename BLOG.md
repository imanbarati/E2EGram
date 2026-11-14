# E2EGram Blog + Grokipedia integration

Last updated: 2026-11-14

## Goal

1. **In-app** entry points from E2EGram settings:
   - **Open Grokipedia** → https://grokipedia.com (external browser / Custom Tab)
   - **Open E2EGram blog** → configurable Astro site on Cloudflare (default placeholder `https://e2egram-blog.pages.dev`)

2. **Standalone global blog** (separate from the APK):
   - Stack: **Astro** + **Cloudflare Workers** + **EmDash** (Cloudflare’s Astro-native CMS) or Markdown content collections
   - Full theme + CMS admin for posts, pages, media
   - Edge-hosted (D1 + R2 when using EmDash)

Grokipedia is an AI-written encyclopedia (xAI). We only **open** it; we do not fork or rehost its content (license / ToS).

## App integration (settings)

| Setting button | Action |
|----------------|--------|
| Open Grokipedia | `ACTION_VIEW` https://grokipedia.com |
| Open E2EGram blog | `ACTION_VIEW` blog base URL (placeholder until deployed) |

Later: Custom Tabs, offline cache of own blog posts, configurable base URL in `MgAccountConfig`.

## Recommended blog stack

| Layer | Choice |
|-------|--------|
| Framework | Astro 5/6+ |
| Host | Cloudflare Workers (`@astrojs/cloudflare`) |
| CMS | **EmDash** (Astro integration, admin at `/_emdash/admin`, D1/R2) |
| Alt CMS | Markdown content collections only (no admin UI) |
| Theme | Fork any Astro blog theme; keep tokens in one CSS theme file |

Scaffold (separate repo, e.g. `imanbarati/e2egram-blog`):

```bash
npm create cloudflare@latest e2egram-blog -- --framework=astro
cd e2egram-blog
# add EmDash per Cloudflare / EmDash docs
# or: src/content/blog/*.md with content collections
npx wrangler deploy
```

Point the app’s blog button at the Workers/Pages URL after first deploy.

## What not to do

- Do not vendor Grokipedia HTML into the APK
- Do not claim Grokipedia articles are E2EGram content
- Do not block Telegram CI on blog deploys — blog is a separate pipeline
