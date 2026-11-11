# Upstream tracking

| Date | Mercurygram | Telegram | Notes |
|------|-------------|----------|-------|
| 2026-11-11 | 13.2.1.1 (branch tip) | 13.2.1 | Patches regenerated; GroupCreate uses addWeakObserver; hideTranslateBar present in MgAccountConfig |
| 2026-10-27 | ~13.0.x | 13.0.x | Initial E2E-only scaffold |

Workflow always checks out `Mercurygram/Mercurygram` @ `Mercurygram` branch tip, then applies `*.java.patch`.
After large upstream jumps, re-run dry-run `patch -p1` against a fresh checkout and refresh hunks if needed.
