# RimRunnerArkansas

Unofficial Arkansas HS boys basketball ratings. Not affiliated with the AAA (Arkansas Activities Association) or MaxPreps.
Status: **Live** at https://rimrunnerarkansas.web.app/ (Firebase Hosting, since 2026-10-18). Accounts, picks and follows sync through the
site's own Firebase project (rimrunnerarkansas). This repo's GitHub Pages copy is only a backup.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's Firebase project (accounts, picks, follows).

## Nightly scores

`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 07:30 and 15:00 UTC
(1:30 am / 9:00 am Central standard time) and on manual dispatch. Before the Arkansas boys season opens
(`SEASON_START` in index.html = Mon Oct 19, 2026, the AAA first contest date for schools without football) the script makes no requests.
Finals before Mon Nov 9, 2026 that involve a football school (team flag `football`) are ignored (AAA first contest date for football schools).
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Arkansas scores-by-date page
(team-page fallback), matches them to the schedule by MaxPreps team id + date, records them in `data/results.json` and
bakes scores and W-L records into `index.html`. The page recomputes Elo from those finals. It commits only when data
changed. The live Firebase site is then published from the bot's computer (boxtools/cf_sync.sh in the build folder); Arkansas has no
Cloudflare Pages project, so the workflow's Cloudflare steps are skipped.
