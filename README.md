# Daniel Pradilla Projects

Static source for [danielpradilla.info/projects](https://www.danielpradilla.info/projects/).

The page is plain HTML and CSS with no build step. It combines the visual language of the Daniel Pradilla blog and app style guide with a screenshot-led project index.

For project-selection rules, screenshot preparation, visual constraints, verification, and deployment guidance, see [AGENTS.md](AGENTS.md).

The Braided Crowns of Europe is published at [projects/european-monarchies-timeline/](https://www.danielpradilla.info/projects/european-monarchies-timeline/), with the timeline's `www/` contents served directly at that URL.

## Project URLs

All eleven catalogued projects are published under `/projects/<slug>/`: `d3clock`, `diminishing-returns`, `bivariate-climate-map`, `choplifter`, `swiss-commutes`, `classification-visualization`, `stylometric-analysis`, `european-monarchies-timeline`, `flipasio`, `uk-music-cities`, and `leboncoin`. The clock stays pinned first; the rest run newest to oldest by repository creation date.

The domain-root redirect rules are maintained in `ops/project-redirects.htaccess`; merge them into the site's existing `.htaccess`, not the catalog directory's file. Migrated URLs use permanent HTTP 308 redirects, preserving methods, query strings and deep links. The older monarchies redirect remains permanent HTTP 301. Flipasio's former `/flipasio/web/` prefix is removed, and its dictionary is now `web/data/calculator_words.txt`. The historical Geneva and Swiss border commute aliases lead directly to Swiss Commutes.

EchoTree was removed from the catalog. EchoSpring, its accounts and the existing `/echotree/` redirects remain unchanged.

### Deployment targets

Publish only this catalog's `index.html`, `styles.css`, `images/` and `.htaccess` to `~/danielpradilla.info/projects/`. Do not mirror or delete that directory: it also holds the applications.

Swiss Commutes uses its checked `out/` with `/projects/swiss-commutes` as its Next base path. Its deployed private train state and timetable were retained; the traffic collector, train collector and timetable updater cron paths now point into `projects/swiss-commutes/`.

UK Music Cities uses `npm run build:deploy` in its `interactive/` directory, then publishes `dist/` and the PHP `api/` to `projects/uk-music-cities/`. The score database remains outside the web root. Existing browser player cookies were scoped to the old path, so returning players may receive a new identity; stored leaderboard scores are retained.

Flipasio publishes the contents of `web/` directly to `projects/flipasio/`, including `web/data/`. The other static projects retain their existing relative asset paths.


## Local preview

From this directory:

```sh
python3 -m http.server 8778 --bind 127.0.0.1
```

Open `http://127.0.0.1:8778/`.

## Bivariate climate map

An interactive map of annual mean maximum temperature and precipitation for seven continents and World from 1950 to 2025. Continents use TerraClimate’s native approximately 4 km grid; World samples every sixth native cell, approximately 28 km at the equator. Each region has fixed historical colour thresholds.

Published at `/projects/bivariate-climate-map/`. The screenshot is `images/bivariate-climate-map.jpg`. The railway clock remains first; the climate map follows it.

The card links to the app’s source-code section, which now links to the [public GitHub repository](https://github.com/danielpradilla/bivariate-climate-map) and its explanatory notebook. Credits for Milan Janosov’s method, earlier adaptations, TerraClimate, Natural Earth and D3 remain in the app.

The title selector switches between seven continents and World; Europe remains the initial view. The legend explanations are collapsed under “How to use.” The city selector is in “Inspect a location,” with “Click on the map or select” as its label. Selecting a city adds a three-second ripple around its marker, disabled for reduced motion. The map supports wheel zoom, trackpad pinch and two-finger touch pinch; scrolling outside the map still scrolls the page.

### Static deployment

Upload the climate assets before its `index.html`, then upload the index screenshot before the projects homepage. Preserve unrelated files; do not use a deleting sync.

When replacing the app JavaScript, stylesheet or card screenshot, update their `v` query parameters in the corresponding HTML. Live verification caught cached older assets even after the replacement files had been uploaded.

All climate-map directories require mode `755` and files require `644`, including `vendor/`. Its script tags opt out of Cloudflare Rocket Loader to preserve D3 loading order. `scripts/.htaccess` publishes `build_data.py` and `build_regions.py` as plain-text source instead of CGI endpoints.

Live verification covered the index card and screenshot, mobile navigation, 1950 climate readings, playback through 1952, and source downloads. The browser environment blocked analytics requests; the map and its local assets loaded successfully.

The interface update was also checked with wheel zoom, Control-wheel trackpad-pinch input, two-finger touch input, normal page scrolling outside the map, colour selection, collapsed help and city selection. The deployed map passed wheel and touch-pinch checks.

The continent extension contains 608 annual frames: 76 years for each of eight views. Verification checked every frame’s hashes, raw/smoothed validity masks, 12 monthly source dates and pooled class thresholds. Independent NCSS point requests confirmed all 12 monthly values and the resulting 2025 annual readings for each of the seven new views, including the South Pole. Every view rendered 1950 and 2025 locally; World playback displayed all 76 years in order and stopped at 2025. Live checks covered every region, historical playback, 320px layout, ripple anchoring through zoom and reduced-motion suppression. Published frame indexes and the first/last raw and smoothed assets matched the local files.

The app documents the 2024 Americas comparison with Janosov’s percentile rule. On revised TerraClimate v1.1 data, annual-only and pooled historical thresholds produce identical classes for 50.1% of South American cells and 80.9% of North American cells. These figures compare threshold baselines, not pixels in the original published image. Missing coastal cells remain explicitly unavailable without substituting nearby readings.
