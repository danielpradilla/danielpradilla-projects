# Rebuilding `/projects`

Rebuild this page as a static HTML/CSS site. Do not add JavaScript, a framework, a package manager, or a build step unless the requested behavior cannot be done with HTML and CSS.

## Outcome

Publish a responsive project index at `https://www.danielpradilla.info/projects/`. Every project card must contain:

- A screenshot that links to the deployed project.
- The project name and a short category label.
- One sentence taken from the opening description in its README.
- A link to the deployed project.
- A link to its GitHub repository.

Both the page header and footer must link to `/blog/`.

## Visual sources of truth

Combine these references rather than copying either one literally:

1. The active blog at `https://www.danielpradilla.info/blog/` supplies the masthead/footer language, restrained typography, and `#fffdf9` background.
2. `https://github.com/danielpradilla/danielpradilla-app-style` supplies the app tokens and component rules: system sans typography, light weights, square edges, hairline borders, muted text, no decorative shadows, and color reserved for interaction.
3. `https://www.habibicode.org/` is only a layout reference for the two-column screenshot-card index.

Keep the canonical tokens in `styles.css`: `--page`, `--paper`, `--paper-soft`, `--ink`, `--muted`, `--secondary`, `--rule`, and `--blue`.

## Project inventory

The current page includes deployed top-level projects that have a matching public repository and a usable README description:

| Project | Deployed path | Repository |
| --- | --- | --- |
| Swiss Railway Clock in D3.js | `/d3clock/` | `danielpradilla/d3clock` |
| The Braided Crowns of Europe | `/european-monarchies-timeline/www/` | `danielpradilla/european-monarchies-timeline` |
| Classification Model Visualizer | `/classification-visualization/` | `danielpradilla/classification-visualization` |
| Stylometric Analysis | `/stylometric-analysis/` | `danielpradilla/stylometric-analysis` |
| EchoTree | `/echotree/` | `danielpradilla/echotree` |
| flipasio | `/flipasio/web/` | `danielpradilla/flipasio` |
| Le Bon Coin Query | `/leboncoin/` | `danielpradilla/leboncoin` |

Stylometric Analysis is intentionally password-protected. EchoTree intentionally redirects to its login screen. Do not describe either as publicly accessible; “published” is accurate.

When refreshing the inventory:

1. Inspect top-level directories in the configured `danielpradilla.info` document root. Keep server addresses and credentials outside the repository.
2. Compare directory names with public repositories returned by the GitHub API for `danielpradilla`.
3. Include a project only when its deployed entry point, repository, README description, and representative screenshot can all be identified.
4. Do not include infrastructure, private file tools, duplicate/legacy directories, third-party checkouts, or hosted apps without a public repository merely to increase the count.

## Screenshots

Prefer, in order:

1. A current screenshot already maintained by the project.
2. A project screenshot used as the featured image in the blog.
3. A fresh browser screenshot of the deployed app or a locally served copy of its public assets.

Store final images in `images/`, use descriptive lowercase filenames, and normalize them to a 1000-pixel-wide JPEG at 80% quality:

The Swiss Railway Clock card is intentionally a live, non-interactive iframe instead of a screenshot so its hands show the current time. Keep a full-card link over the iframe so the project remains easy to open.

Use the available image-processing workflow to export a proportional, 1000-pixel-wide JPEG at 80% quality. Do not commit temporary captures or original full-resolution assets.

Set accurate `width`, `height`, and useful `alt` attributes. Cards crop screenshots to `16 / 9` with `object-fit: cover`; choose an image whose important content survives that crop.

## Required structure

- Keep `index.html`, `styles.css`, and `images/` as the complete deployable artifact.
- Use semantic `header`, `nav`, `main`, `section`, `article`, and `footer` elements.
- Preserve the keyboard skip link and visible `:focus-visible` treatment.
- Keep the desktop grid at two columns and collapse it to one column below `760px`.
- Use root-relative links for projects and the blog so local paths remain valid after deployment.
- Keep GitHub links explicit on every card.
- Respect `prefers-reduced-motion`.

## Verification

Serve the workspace locally from its parent directory:

```sh
python3 -m http.server 8778 --bind 127.0.0.1
```

Verify at desktop and mobile widths:

- Every card image loads.
- The card count matches the eyebrow count.
- The mobile grid has one column and `document.documentElement.scrollWidth <= window.innerWidth`.
- Header and footer both contain working Blog and GitHub links.
- Focus states are visible and image alt text is meaningful.
- Project and GitHub destinations are correct. Treat an expected `401` or login redirect as valid only for the two protected projects noted above.

## Deployment

Deploy only this directory through the configured `danielpradilla.info` deployment workflow and preserve unrelated files. Do not put hostnames, usernames, credentials, or machine-specific paths in this repository.

If the configured workflow uses `rsync`, do not use `--delete`. After deployment, verify `/projects/`, `styles.css`, every image, every project URL, and every GitHub repository. Keep remote writes confined to the domain's `/projects/` directory.
