# Personal app hub: notes for Claude

Ad-free personal apps for one user (Android phone, Chrome, US / Eastern time).
Built and maintained by Claude. This repo is public: never commit personal data,
locations, tokens or keys.

## Structure
- `/` launcher page (`index.html`). Add a tile here for every new app.
- `/<app>/` one folder per app, fully self-contained (own `index.html`, `manifest.webmanifest`, `sw.js`, icons).
- `/helper/` (planned) Cloudflare Worker source; deployed by GitHub Actions using repo secrets
  `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID`.
- Served by GitHub Pages from `main`, root, at https://bobbyee358-dev.github.io/
- `.nojekyll` is present: files are served as-is.

## Conventions
- Plain static HTML/CSS/JS, no build step. Pin any CDN library to an exact version.
- All apps share one origin, so `localStorage` is shared: prefix every key with the app name (`nimbus.loc`).
- Service workers live inside each app folder and are scoped to it. Do NOT add a service worker at the root:
  its scope would cover every app.
- Service workers cache only the app shell. Never cache map tiles or API responses offline
  (OpenStreetMap's tile policy forbids offline tile storage).
- Bump the `CACHE` name in an app's `sw.js` when its shell files change.
- Show source attribution on screen for every data source.
- Never show "no alerts" or similar all-clear states when a request failed; show the failure.
- Shared look: tokens from `radar/index.html` (`--bg`, `--surface`, `--accent` ...), system font, light + dark.

## Data sources (checked Oct 2026)
- Radar default ("filtered"): NOAA MRMS Seamless Hybrid-Scan Reflectivity via IEM tiles. Latest: `q2-hsr`;
  loop frames: `mrms::lcref-YYYYMMDDHHMI` (UTC, even minutes). MRMS dual-pol QC removes birds, insects,
  clutter and AP. Why: the raw NEXRAD mosaic showed a metro-wide false "rain" bloom over Atlanta on a dry
  night (user report, Oct 8 2026). Frames whose tiles all fail are skipped in the loop.
- Radar "raw" (toggle): IEM NEXRAD mosaic `nexrad-n0q-900913[-mXXm]` (confirmed working on the phone);
  only light clutter filtering. Valid time: `/data/gis/images/4326/USCOMP/n0q_0.json`.
- Tile base: `https://{mesonet1|2|3}.agron.iastate.edu/cache/tile.py/1.0.0/<layer>/{z}/{x}/{y}.png`
  (layer list: GET `/cache/tile.py/1.0.0/`, all EPSG:3857).
- Forecast + geocoding: Open-Meteo (free for non-commercial use, attribution required).
- Alerts + place names: api.weather.gov (works from the browser; confirmed on the phone).
- Base map: tile.openstreetmap.org (attribution required, no offline/bulk use).
- Avoid RainViewer: free tier cut to zoom 7, no nowcast, since Jan 1 2026.

## Working environment limits
- Claude's sandbox cannot reach github.io, api.weather.gov, IEM or CDNs. Test pages locally with
  Playwright and mocked responses; the user verifies live behavior with screenshots.
- Claude can push to repos here but cannot create repos, change repo settings (e.g. Pages) or add secrets.
- New repos: after the user creates one, pushes may be refused ("Claude doesn't have GitHub access") until the
  Claude GitHub App covers it. Ask the user to check github.com/settings/installations (Claude, Configure,
  Repository access) and, if needed, re-link via claude.ai/customize/connectors (GitHub Integration).
  The claude.ai page's "Check repository status" box confirms access for a given repo.
- New Pages sites: a build may not start until a commit is pushed after Pages is switched on. Push an
  empty commit if Settings, Pages shows no "Visit site" button after a few minutes.
- The old repo bobbyee358-dev/Nimbus now only forwards /Nimbus/ to /radar/. Do not develop there.
- On the user's Android phone, claude.ai links open in the Claude app, which breaks GitHub re-linking.
  Fix: Android Settings, Apps, Claude, Open by default, turn off "Open supported links" during re-linking.

## Open items
See `BACKLOG.md`.
